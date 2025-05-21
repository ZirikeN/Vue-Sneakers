<template>
  <div v-if="cart.length" class="flex flex-col flex-1 gap-4 overflow-y-scroll" v-auto-animate>
    <CartItem
      v-for="item in cart"
      :key="item.id"
      :title="item.title"
      :imageUrl="item.imageUrl"
      :price="item.price"
      @onClickRemove="() => removeCart(item)"
    ></CartItem>
  </div>
</template>

<script setup>
import { inject } from 'vue'

import CartItem from './CartItem.vue'

const { cart, removeCart } = inject('cart')
</script>
