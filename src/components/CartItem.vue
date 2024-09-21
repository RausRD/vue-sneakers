<script setup>
const emit = defineEmits(['onClickRemove']);

defineProps({
  id: Number,
  title: String,
  imageUrl: String,
  price: Number
});
</script>

<template>
  <div class="flex items-center gap-4 p-4 border rounded-xl border-slate-200">
    <img :src="imageUrl" alt="Sneaker" class="w-16 h-16" />
    <div class="flex flex-col flex-1">
      <p>{{ title }}</p>
      <div class="flex justify-between mt-2">
        <b class="flex-1">{{ price }} руб.</b>
        <img
          @click="emit('onClickRemove')"
          class="transition cursor-pointer opacity-40 hover:opacity-100"
          src="/close.svg"
          alt="Close"
        />
      </div>
    </div>
  </div>
</template>
