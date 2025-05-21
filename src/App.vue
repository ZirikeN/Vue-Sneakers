<template>
  <Drawer v-if="drawerOpen" :totalPrice="totalPrice" @createOrder="createOrder"></Drawer>

  <div class="bg-white w-4/5 m-auto rounded-2xl shadow-xl mt-14">
    <Header @openDrawer="openDrawer" :totalPrice="totalPrice"></Header>

    <div class="m-10">
      <router-view></router-view>
    </div>
  </div>
</template>

<script setup>
import { ref, watch, provide, computed } from 'vue'
import axios from 'axios'

import Header from './components/Header.vue'
import Drawer from './components/Drawer.vue'

const cart = ref([])

const drawerOpen = ref(false)

const totalPrice = computed(() => {
  return cart.value.reduce((acc, item) => acc + item.price, 0)
})
const openDrawer = () => {
  drawerOpen.value = true
}
const closeDrawer = () => {
  drawerOpen.value = false
}

const addToCart = (item) => {
  cart.value.push(item)
  item.isAdded = true
  console.log(cart.value)
}

const removeCart = (item) => {
  cart.value.splice(cart.value.indexOf(item), 1)
  item.isAdded = false
}

const createOrder = async () => {
  try {
    const { data } = await axios.post(`https://e1c2179a26cd8609.mokky.dev/orders`, {
      items: cart.value,
      totalPrice: totalPrice.value,
    })

    cart.value = []
    console.log('post')
    items.value.forEach((item) => (item.isAdded = false))

    return data
  } catch (e) {
    console.log(e)
  }
}


watch(
  cart,
  () => {
    localStorage.setItem('cart', JSON.stringify(cart.value))
  },
  { deep: true },
)

provide('cart', {
  openDrawer,
  closeDrawer,
  cart,
  addToCart,
  removeCart,
})
</script>

<style scoped></style>
