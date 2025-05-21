<template>
  <div class="flex items-center border border-slate-100 p-4 rounded-2xl gap-4">
    <img class="w-16 h-16" :src="imageUrl" alt="Sneaker" />

    <div class="flex justify-between flex-col">
      <p>{{ title }}</p>

      <div class="flex justify-between mt-2">
        <b>{{ price }}</b>
        <img
          @click="$emit('onClickRemove')"
          class="opacity-30 cursor-pointer hover:opacity-100 transition"
          src="/close.svg"
          alt="close"
        />
      </div>
    </div>
  </div>
</template>

<script setup>
defineProps({
  id: Number,
  title: String,
  imageUrl: String,
  price: Number,
})

const emit = defineEmits(['onClickRemove'])
</script>
