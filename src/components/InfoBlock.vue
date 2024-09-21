<script setup>
defineProps({
  title: String,
  description: String,
  imageUrl: String
});
</script>

<template>
  <div class="flex flex-col items-center mx-auto text-center w-72">
    <img :src="imageUrl" alt="Image" height="80" width="80" />
    <h2 class="mt-4 text-2xl font-medium">{{ title }}</h2>
    <p class="mt-2 text-gray-400">{{ description }}</p>
  </div>
</template>
