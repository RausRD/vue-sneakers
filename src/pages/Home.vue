<script setup>
import CardList from '@/components/CardList.vue';
import axios from 'axios';
import { inject, onMounted, reactive, ref, watch } from 'vue';

const filters = reactive({
  sortBy: 'title',
  searchQuery: ''
});
const items = ref([]);
const { cart, addToCart, removeFromCart } = inject('cart');

const onClickAddPlus = (item) => {
  if (!item.isAdded) {
    addToCart(item);
  } else {
    removeFromCart(item);
  }
};

const onChangeSelect = (e) => {
  filters.sortBy = e.target.value;
};

const onChangeSearchInput = (e) => {
  filters.searchQuery = e.target.value;
};

const addToFavorite = async (item) => {
  try {
    if (!item.isFavorite) {
      const obj = {
        item_id: item.id
      };
      item.isFavorite = true;
      const { data } = await axios.post(`https://96412ad6d18eb9e5.mokky.dev/favorites`, obj);
      item.favoriteId = data.id;
    } else {
      item.isFavorite = false;
      await axios.delete(`https://96412ad6d18eb9e5.mokky.dev/favorites/${item.favoriteId}`);
      item.favoriteId = null;
    }
  } catch (error) {
    console.log(error);
  }
};
const fetchFavorites = async () => {
  try {
    const { data: favorites } = await axios.get(`https://96412ad6d18eb9e5.mokky.dev/favorites`);
    items.value = items.value.map((item) => {
      const favorite = favorites.find((favorite) => favorite.item_id === item.id);

      if (!favorite) {
        return item;
      }

      return {
        ...item,
        isFavorite: true,
        favoriteId: favorite.id
      };
    });
  } catch (error) {
    console.log(error);
  }
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
    items.value = data.map((obj) => ({
      ...obj,
      isFavorite: false,
      favoriteId: null,
      isAdded: false
    }));
  } catch (error) {
    console.log(error);
  }
};

onMounted(async () => {
  const localCart = localStorage.getItem('cart');
  cart.value = localCart ? JSON.parse(localCart) : [];

  await fetchItems();
  await fetchFavorites();
  items.value = items.value.map((item) => ({
    ...item,
    isAdded: cart.value.some((cartItem) => cartItem.id === item.id)
  }));
});

watch(filters, fetchItems);
watch(cart, () => {
  items.value = items.value.map((item) => ({
    ...item,
    isAdded: false
  }));
});
</script>

<template>
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
    <CardList :items="items" @add-to-favorite="addToFavorite" @add-to-cart="onClickAddPlus" />
  </div>
</template>
