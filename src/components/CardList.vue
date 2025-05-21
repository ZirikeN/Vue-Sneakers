<template>
  <div class="grid grid-cols-4 gap-10" v-auto-animate>
    <Card
      v-for="item in items"
      :title="item.title"
      :id="item.id"
      :imageUrl="item.imageUrl"
      :price="item.price"
      :key="item.id"
      :isFavorite="item.isFavorite"
      :onClickFavorite="isFavorites ? null : () => emit('addToFavorite', item)"
      :onClickAdd="isFavorites ? null : () => emit('addToCart', item)"
      :isAdded="item.isAdded"
    ></Card>
  </div>
</template>

<script setup>
import Card from './Card.vue'

defineProps({
  items: Array,
  isFavorites: Boolean,
})

const emit = defineEmits(['addToFavorite', 'addToCart'])
</script>
