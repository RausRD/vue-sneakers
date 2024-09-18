<script setup>
import { onMounted, reactive, ref, watch } from 'vue';
import axios from 'axios';
import CardList from './components/CardList.vue';
import Header from './components/Header.vue';

const items = ref([]);
const filters = reactive({
  sortBy: 'title',
  searchQuery: ''
});

const onChangeSelect = (e) => {
  filters.sortBy = e.target.value;
};

const onChangeSearchInput = (e) => {
  filters.searchQuery = e.target.value;
};

const fetchItems = async () => {
  try {
    const params = {
      sortBy: filters.sortBy
    };
    if (filters.searchQuery) {
      params.title = `*${filters.searchQuery}*`;
    }
    const { data } = await axios.get(`https://96412ad6d18eb9e5.mokky.dev/items`, {
      params
    });
    items.value = data;
  } catch (error) {
    console.log(error);
  }
};

onMounted(fetchItems);

watch(filters, fetchItems);
</script>

<template>
  <!-- <Drawer/> -->
  <div class="w-4/5 m-auto bg-white shadow-xl rounded-xl mt-14">
    <Header />

    <div class="p-10">
      <div class="flex items-center justify-between">
        <h2 class="mb-8 text-3xl font-bold">Все кроссовки</h2>
        <div class="flex gap-4">
          <select @change="onChangeSelect" class="px-3 py-2 border rounded-md outline-none">
            <option value="name">По названию</option>
            <option value="price">По цене (дешевые)</option>
            <option value="-price">По цене (дорогие)</option>
          </select>
          <div class="relative">
            <img src="/search.svg" alt="Search" class="absolute left-4 top-3" />
            <input
              @input="onChangeSearchInput"
              class="py-2 pr-4 border rounded-md outline-none pl-11 focus:border-gray-400"
              type="text"
              placeholder="Поиск..."
            />
          </div>
        </div>
      </div>
      <div class="mt-10">
        <CardList :items="items" />
      </div>
    </div>
  </div>
</template>
