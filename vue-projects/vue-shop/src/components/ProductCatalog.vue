<template>
    <div class="container mx-auto p-4">
      <div class="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 lg:grid-cols-4 gap-6">
        <div v-for="product in products" :key="product.id"
          class="bg-white shadow rounded-lg p-4 flex flex-col items-center transform transition-all duration-300 hover:scale-105 hover:shadow-xl">
          <img :src="product.image" :alt="product.name" class="w-32 h-32 object-cover mb-4 rounded-md"
            @click="goToProduct(product)" />
          <h2 class="text-lg font-semibold mb-2">{{ product.name }}</h2>
          <p class="text-gray-500 text-sm mb-2">{{ product.description }}</p>
          <span class="text-green-800 font-bold text-lg">
            {{ formatPrice(product.price) }} ₽
          </span>
          <div class="flex space-x-2 mt-4">
            <button @click="goToProduct(product)"
              class="bg-green-500 text-white px-4 py-2 rounded-md hover:bg-green-600 transition w-full">
              Подробнее
            </button>
            <button @click="addToCart(product)"
              class="bg-green-500 text-white px-4 py-2 rounded-md hover:bg-green-600 transition w-full">
              В корзину
            </button>
          </div>
        </div>
      </div>
    </div>
  </template>
  
  <script lang="ts">
  import { defineComponent } from 'vue';
  import { useCartStore } from '@/store/cartStore';
  import туфлеботинкокроссовки from '@/assets/туфлеботинкокроссовки.jpeg';
  import airmax from '@/assets/airmax.jpeg';
  import nb530 from '@/assets/nb530.webp';
  import airforce from '@/assets/airforce.webp';
  import samba from '@/assets/samba.webp';


  
  export default defineComponent({
    name: 'ProductCatalog',
    data() {
      return {
        products: [
          {
            id: 1,
            name: 'Nike Air Max 95',
            description: 'Кроссовки для забива)',
            price: 14999,
            image: airmax,
          },
          {
            id: 2,
            name: 'New Balance 530',
            description: 'Кроссовки нефорские',
            price: 5000,
            image: nb530,
          },
          {
            id: 3,
            name: 'Nike Air Force 1 Low',
            description: 'Форсы зафорсили :)',
            price: 1000,
            image: airforce,
          },
          {
            id: 4,
            name: 'Addidas Samba',
            description: 'Кроссовки для борьбы)',
            price: 25000,
            image: samba,
          },
          {
            id: 5,
            name: 'Туфле-ботинко-кроссовки',
            description: 'Папаня одобрил',
            price: 999999999999,
            image: туфлеботинкокроссовки,
          },
        ],
      };
    },
    methods: {
      addToCart(product: { id: number, name: string, description: string, price: number, image: string }) {
        const cartStore = useCartStore();
        console.log(product)
        cartStore.addToCart(product);
      },
      formatPrice(price: number): string {
        return price.toLocaleString('ru-RU');
      },
      goToProduct(product: { id: number }) {
        this.$router.push(`/product/${product.id}`);
      },
    },
  });
  </script>
  