<template>
    <Header />
    <div class="container mx-auto py-8">
        <div v-if="product">
            <div class="flex flex-col lg:flex-row items-center">
                <div class="flex-1 mb-8 lg:mb-0 lg:w-1/2">
                    <img :src="product.image" :alt="product.name"
                        class="w-full h-auto object-cover rounded-lg shadow-md" />
                </div>

                <div class="flex-1 lg:ml-8">
                    <h1 class="text-3xl font-bold text-gray-800 mb-4">{{ product.name }}</h1>
                    <p class="text-gray-600 mb-6">{{ product.description }}</p>

                    <div class="bg-gray-100 p-4 rounded-lg shadow-md mb-6">
                        <h2 class="text-lg font-semibold text-gray-700 mb-2">Характеристики:</h2>
                        <ul class="list-disc list-inside text-gray-600">
                            <li v-for="(spec, index) in product.specs" :key="index">
                                {{ spec }}
                            </li>
                        </ul>
                    </div>

                    <div class="text-xl font-semibold text-green-800 mb-6">
                        {{ formattedPrice(product.price) }} ₽
                    </div>

                    <button @click="addToCart(product)"
                        class="px-6 py-3 bg-green-600 text-white font-semibold rounded-lg hover:bg-green-700 transition duration-300">
                        Добавить в корзину
                    </button>
                </div>
            </div>
        </div>
        <div v-else>
            <p>Товар не найден.</p>
        </div>
    </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";
import Header from "@/components/Header.vue";
import { useCartStore } from "@/store/cartStore";
import туфлеботинкокроссовки from '@/assets/туфлеботинкокроссовки.jpeg';
import airmax from '@/assets/airmax.jpeg';
import nb530 from '@/assets/nb530.webp';
import airforce from '@/assets/airforce.webp';
import samba from '@/assets/samba.webp';

interface Product {
    id: number;
    name: string;
    description: string;
    price: number;
    image: string;
    specs: string[];
}

export default defineComponent({
    name: "Product",
    components: { Header },
    data() {
        return {
            product: null as Product | null,
        };
    },
    mounted() {
        this.fetchProduct();
    },
    methods: {
        fetchProduct(): void {
            const products: Product[] = [
                {
                    id: 1,
                    name: "Nike Air Max 95",
                    description: "Кроссовки для забива)",
                    price: 14999,
                    image: airmax,
                    specs: [
                    ]
                },
                {
                    id: 2,
                    name: "New Balance 530",
                    description: "Кроссовки нефорские",
                    price: 5000,
                    image: nb530,
                    specs: [
                    ]
                },
                {
                    id: 3,
                    name: "Nike Air Force 1 Low",
                    description: "Форсы зафорсили :)",
                    price: 1000,
                    image: airforce,
                    specs: [
                    ]
                },
                {
                    id: 4,
                    name: "Addidas Samba",
                    description: "Кроссовки для борьбы)",
                    price: 25000,
                    image: samba,
                    specs: [
                    ]
                },
                {
                    id: 5,
                    name: "Туфле-ботинко-кроссовки",
                    description: "Папаня одобрил",
                    price: 999999999999,
                    image: туфлеботинкокроссовки,
                    specs: [
                    ]
                }
            ];

            const paramId = Array.isArray(this.$route.params.id)
                ? this.$route.params.id[0]
                : this.$route.params.id;

            // Преобразуем параметр id в число
            const productId = parseInt(paramId, 10);

            if (isNaN(productId)) {
                console.error("Некорректный ID товара!");
                return;
            }

            this.product = products.find((prod) => prod.id === productId) || null;

            if (!this.product) {
                console.error("Товар с таким id не найден!");
            }
        },

        formattedPrice(price: number): string {
            return price.toLocaleString("ru-RU");
        },
        addToCart(product: Product): void {
            const cartStore = useCartStore();
            cartStore.addToCart(product);
        },
    },
});
</script>
