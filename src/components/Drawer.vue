<script setup>
import CartItemList from './CartItemList.vue'
import DrawerHead from './DrawerHead.vue'
</script>

<template>
  <div class="fixed top-0 left-0 z-10 w-full h-full bg-black opacity-70"></div>
  <div class="fixed top-0 right-0 z-20 flex flex-col justify-between h-full p-8 bg-white w-96">
    <DrawerHead />
    <CartItemList />
    <div class="flex flex-col gap-4 mt-7">
      <div class="flex gap-2">
        <span>Итого:</span>
        <div class="flex-1 border-b border-dashed"></div>
        <b>12900 руб.</b>
      </div>
      <div class="flex gap-2">
        <span>Налог 5%:</span>
        <div class="flex-1 border-b border-dashed"></div>
        <b>900 руб.</b>
      </div>
      <button
        disabled
        class="w-full py-3 mt-4 text-white transition cursor-pointer rounded-xl bg-lime-500 hover:bg-lime-600 active:bg-lime-700 disabled:bg-slate-300"
      >
        Оформить заказ
      </button>
    </div>
  </div>
</template>
