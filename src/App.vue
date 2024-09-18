<script setup>
import CardList from './components/CardList.vue'
import Drawer from './components/Drawer.vue'
import Header from './components/Header.vue'
</script>

<template>
  <!-- <Drawer/> -->
  <div class="w-4/5 m-auto bg-white shadow-xl rounded-xl mt-14">
    <Header />

    <div class="p-10">
      <div class="flex items-center justify-between">
        <h2 class="mb-8 text-3xl font-bold">Все кроссовки</h2>
        <div class="flex gap-4">
          <select class="px-3 py-2 border rounded-md outline-none">
            <option>По названию</option>
            <option>По цене (дешевые)</option>
            <option>По цене (дорогие)</option>
          </select>
          <div class="relative">
            <img src="/search.svg" alt="Search" class="absolute left-4 top-3" />
            <input
              class="py-2 pr-4 border rounded-md outline-none pl-11 focus:border-gray-400"
              type="text"
              placeholder="Поиск..."
            />
          </div>
        </div>
      </div>
      <CardList />
    </div>
  </div>
</template>

<style scoped></style>
