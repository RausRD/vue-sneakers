<script setup>
import Card from './Card.vue'
const onClickAdd = () => {
	alert(111)
}
</script>

<template>
  <div class="grid grid-cols-4 gap-5">
    <Card
      title="Sneakers"
      image-url="/sneakers/sneakers-1.jpg"
      :price="5000"
      :is-added="false"
      :is-favorite="false"
      :onClickAdd="onClickAdd"
    />
  </div>
</template>
