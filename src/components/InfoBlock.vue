<template>
  <div class="flex flex-col w-72 mx-auto items-center">
    <img width="120" height="120" :src="imageUrl" alt="Info image">
    <h2 class="mt-4 text-2xl font-bold text-center">{{ title }}</h2>
    <p class="text-center mt-2 text-slate-500">{{ description }}</p>
  </div>
</template>

<script setup>
defineProps({
  title: String,
  description: String,
  imageUrl: String
})
</script>
