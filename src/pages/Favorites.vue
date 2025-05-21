<template>
  <h2 class="text-3xl font-bold mb-8">Мои Закладки</h2>
  <div class="pb-8">
    <CardList v-if="favorites && favorites.length" :items="favorites" isFavorites></CardList>
    <div v-else class="text-center text-slate-700 text-2xl font-semibold">Закладок нет</div>
  </div>
</template>

<script setup>
import axios from 'axios';
import { onMounted, ref } from 'vue';

import CardList from '../components/CardList.vue';

const favorites = ref([])

onMounted(async () => {
  try {
    const {data} = await axios.get(`https://e1c2179a26cd8609.mokky.dev/favorites?_relations=item`);

    favorites.value = data.map(obj => obj.item)
  } catch (e) {
    console.log(e)
  }
})
</script>
