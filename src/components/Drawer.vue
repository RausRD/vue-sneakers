<script setup>
import { computed, inject, ref } from 'vue';
import CartItemList from './CartItemList.vue';
import DrawerHead from './DrawerHead.vue';
import InfoBlock from './InfoBlock.vue';
import axios from 'axios';
const isCreatingOrder = ref(false);
const orderId = ref(null);

const props = defineProps({
  totalPrice: Number,
  vatPrice: Number
});

const { cart } = inject('cart');

const createOrder = async () => {
  try {
    isCreatingOrder.value = true;
    const { data } = await axios.post('https://96412ad6d18eb9e5.mokky.dev/orders', {
      items: cart.value,
      totalPrice: props.totalPrice.value
    });
    cart.value = [];
    data.value = data.id;
  } catch (error) {
    console.log(error);
  } finally {
    isCreatingOrder.value = false;
  }
};

const cartIsEmpty = computed(() => cart.value.length === 0);
const disabledCartButton = computed(() => isCreatingOrder.value || cartIsEmpty.value);
</script>

<template>
  <div class="fixed top-0 left-0 z-10 w-full h-full bg-black opacity-70"></div>
  <div class="fixed top-0 right-0 z-20 flex flex-col justify-between h-full p-8 bg-white w-96">
    <DrawerHead />
    <div v-if="!totalPrice || orderId" class="flex items-center h-full">
      <InfoBlock
        v-if="!totalPrice && !orderId"
        title="Корзина пустая"
        description="Добавьте хотя бы одну пару кросовок, чтобы сделать заказ"
        image-url="/package-icon.png"
      />
      <InfoBlock
        v-if="orderId"
        title="Заказ оформлен"
        :description="`Ваш заказ #${orderId} скоро будет передан курьерской доставке`"
        image-url="/order-success-icon.png"
      />
    </div>
    <CartItemList />
    <div v-if="totalPrice" class="flex flex-col gap-4 mt-7">
      <div class="flex gap-2">
        <span>Итого:</span>
        <div class="flex-1 border-b border-dashed"></div>
        <b>{{ totalPrice }} руб.</b>
      </div>
      <div class="flex gap-2">
        <span>Налог 5%:</span>
        <div class="flex-1 border-b border-dashed"></div>
        <b>{{ vatPrice }} руб.</b>
      </div>
      <button
        :disabled="disabledCartButton"
        @click="createOrder"
        class="w-full py-3 mt-4 text-white transition cursor-pointer rounded-xl bg-lime-500 hover:bg-lime-600 active:bg-lime-700 disabled:bg-slate-300"
      >
        Оформить заказ
      </button>
    </div>
  </div>
</template>
