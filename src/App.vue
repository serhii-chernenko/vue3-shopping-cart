<script setup>
import { computed, ref } from "vue";

const products = ref([
  { id: 1, name: "Mouse - Logitech MX Master 3S", price: 9220, quantity: 1 },
  { id: 2, name: "Keyboard - Logitech MX Keys", price: 7990, quantity: 1 },
  {
    id: 3,
    name: "WebCam - Logitech HD Pro Webcam C920",
    price: 6890,
    quantity: 1,
  },
]);

const total = computed(() => {
  return Math.round(
    products.value.reduce((acc, product) => acc + product.price, 0) / 100
  );
});

function increaseQuantity(product) {
  product.quantity += 1;
}

function decreaseQuantity(product) {
  product.quantity -= product.quantity <= 1 ? 0 : 1;
}

const isOdd = (index) => index % 2;
</script>
<template>
  <div
    class="flex items-center justify-center dark:text-white h-screen w-screen"
  >
    <div
      class="max-w-3xl w-full border border-gray-50 dark:border-gray-600 shadow-2xl rounded-md"
    >
      <h2 class="text-4xl bg-gray-600 p-8 text-gray-200">Shopping Cart</h2>
      <ul class="dark:text-white p-8">
        <li
          v-for="(product, index) in products"
          :key="product.id"
          class="flex flex-col my-2 w-full odd:bg-gray-100 odd:dark:bg-gray-700 p-4"
        >
          <span class="flex items-center justify-between w-full space-x-3">
            <span class="w-1/3 truncate">{{ product.name }}</span>
            <span>${{ product.price / 100 }}</span>
            <span class="inline-flex items-center justify-between space-x-3">
              <button
                class="bg-blue-400 hover:bg-blue-600 py-1 px-2 rounded-md disabled:cursor-not-allowed"
                :class="{
                  'disabled:bg-gray-200': !isOdd(index),
                  'disabled:dark:bg-gray-900': !isOdd(index),
                  'disabled:bg-white': isOdd(index),
                  'disabled:dark:bg-gray-700': isOdd(index)
                 }"
                @click="decreaseQuantity(product)"
                :disabled="product.quantity <= 1"
              >
                -
              </button>
              <span class="w-12 text-center">{{ product.quantity }}</span>
              <button
                class="bg-blue-400 hover:bg-blue-600 py-1 px-2 rounded-md"
                @click="increaseQuantity(product)"
              >
                +
              </button>
            </span>
            <span class="w-16 text-right">
              ${{ (product.price * product.quantity) / 100 }}
            </span>
          </span>
        </li>
      </ul>
      <p class="flex items-center justify-between text-3xl mt-4 p-8">
        <span>Total:</span>
        <span class="font-bold">${{ total }}</span>
      </p>
    </div>
  </div>
</template>
