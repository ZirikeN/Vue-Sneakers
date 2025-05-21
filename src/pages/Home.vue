<template>
  <div class="flex justify-between">
    <h2 class="text-3xl font-bold mb-8">Все кроссовки</h2>

    <div class="flex items-center gap-8">
      <select
        @change="onChangeSelect"
        class="focus:outline-none py-2 px-4 border-slate-200 hover:border-b hover:border-slate-200 transition"
      >
        <option value="name">По названию</option>
        <option value="price">По цене (дешевые)</option>
        <option value="-price">По цене (дорогие)</option>
      </select>

      <div class="relative">
        <img class="absolute top-3 left-4" src="/search.svg" alt="Search" />
        <input
          @input="onChangeSearchInput"
          type="text"
          placeholder="Поиск..."
          class="border border-slate-300 rounded-md pl-10 pr-4 py-2 transition hover:border-slate-400 outline-none focus:border-slate-400"
        />
      </div>
    </div>
  </div>

  <div class="mt-8 pb-8">
    <CardList :items="items" @addToFavorite="addToFavorite" @addToCart="onclickAddPlus"></CardList>
  </div>
</template>

<script setup>
import axios from 'axios'
import { inject, reactive, watch, ref, onMounted } from 'vue'

import CardList from '../components/CardList.vue'

const {addToCart, removeCart, cart} = inject('cart')

const filters = reactive({
  sortBy: 'title',
  searchQuery: '',
})

const items = ref([])

const onclickAddPlus = (item) => {
  if (!item.isAdded) {
    addToCart(item)
  } else {
    removeCart(item)
  }
}

const onChangeSelect = (event) => {
  filters.sortBy = event.target.value
}

const onChangeSearchInput = (event) => {
  filters.searchQuery = event.target.value
}

const addToFavorite = async (item) => {
  try {
    if (!item.isFavorite) {
      const obj = {
        parentId: item.id,
        item
      }
      item.isFavorite = true

      const { data } = await axios.post(`https://e1c2179a26cd8609.mokky.dev/favorites`, obj)

      item.favoriteId = data.id
    } else {
      item.isFavorite = false

      await axios.delete(`https://e1c2179a26cd8609.mokky.dev/favorites/${item.favoriteId}`)
    }
  } catch (e) {
    console.log(e)
  }
}

const fetchFavorites = async () => {
  try {
    const response = await axios.get(`https://e1c2179a26cd8609.mokky.dev/favorites`)

    items.value = items.value.map((item) => {
      const favoriteItem = response.data.find((favItem) => favItem.parentId === item.id)

      if (!favoriteItem) {
        return item
      }

      return {
        ...item,
        isFavorite: true,
        favoriteId: favoriteItem.id,
      }
    })
  } catch (e) {
    console.log(e)
  }
}

const fetchItems = async () => {
  try {
    const params = {
      sortBy: filters.sortBy,
    }

    if (filters.searchQuery) {
      params.title = `*${filters.searchQuery}*`
    }

    const response = await axios.get(`https://e1c2179a26cd8609.mokky.dev/items`, {
      params,
    })

    items.value = response.data.map((obj) => ({
      ...obj,
      isFavorite: false,
      favoriteId: null,
      isAdded: false,
    }))
  } catch (e) {
    console.log(e)
  }
}

onMounted(async () => {
  cart.value = JSON.parse(localStorage.getItem('cart')) || []

  await fetchItems()
  await fetchFavorites()

  items.value = items.value.map((item) => ({ ...item, isAdded: cart.value.some((i) => i.id === item.id) }))
})

watch(filters, fetchItems)
</script>
