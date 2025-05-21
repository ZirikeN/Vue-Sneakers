<template>
  <div class="fixed top-0 left-0 h-full w-full bg-black z-10 opacity-70"></div>
  <div class="bg-white w-96 h-full fixed top-0 right-0 z-20 p-8 flex flex-col">
    <DrawerHead></DrawerHead>

    <div v-if="!totalPrice" class="flex h-full items-center">
      <InfoBlock title="Корзина пустая" description="Добавьте хотя бы одну пару кроссовок, чтобы сделать заказ." imageUrl="/package-icon.png"></InfoBlock>
    </div>

    <CartListItem></CartListItem>

    <div v-if="totalPrice" class="flex flex-col gap-5 mt-6">
      <div class="flex gap-2">
        <span>Итого:</span>
        <div class="flex flex-1 border-b border-dashed"></div>
        <b>{{ totalPrice.toFixed(2) }} руб.</b>
      </div>

      <div class="flex gap-2">
        <span>Налог 5%:</span>
        <div class="flex flex-1 border-b border-dashed"></div>
        <b>{{ (totalPrice * 0.05).toFixed(2) }} руб.</b>
      </div>

      <button
        @click="$emit('createOrder')"
        :disabled="totalPrice ? false : true"
        class="bg-lime-500 hover:bg-lime-600 active:bg-lime-700 disabled:bg-slate-400 transition py-4 px-8 text-white mt-6 rounded-2xl w-full cursor-pointer"
      >
        Оформить заказ
      </button>
    </div>
  </div>
</template>

<script setup>
import DrawerHead from './DrawerHead.vue'
import CartListItem from './CartListItem.vue'
import InfoBlock from './InfoBlock.vue'

defineProps({
  totalPrice: Number,
})

const emit = defineEmits(['createOrder'])
</script>
