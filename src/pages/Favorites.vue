<script setup>
import CardList from '@/components/CardList.vue';
import axios from 'axios';
import { onMounted, ref } from 'vue';

const favorites = ref([]);

onMounted(async () => {
  try {
    const { data } = await axios.get(
      'https://96412ad6d18eb9e5.mokky.dev/favorites?_relations=items'
    );
    favorites.value = data.map((obj) => obj.item);
  } catch (error) {
    console.log(error);
  }
});
</script>

<template>
  <h2 class="mb-8 text-3xl font-bold">Мои закладки</h2>
  <CardList :items="favorites" is-favorites />
</template>
