<template>
  <div class="min-h-screen">
    <Navbar
      :cartCount="totalItems"
      @open-cart="isCartOpen = true"
      @open-advisor="isAdvisorOpen = true"
    />

    <main>
      <Hero />

      <!-- Shop Section -->
      <section id="shop" class="max-w-7xl mx-auto px-4 py-24">
        <div class="flex flex-col md:flex-row md:items-end justify-between mb-12 gap-8">
          <div>
            <h2 class="text-4xl font-extrabold text-emerald-950 mb-2 uppercase tracking-tighter">
              精準裝備
            </h2>
            <div class="w-20 h-1 bg-orange-600"></div>
          </div>
          <div class="flex flex-wrap gap-2">
            <button
              v-for="cat in CATEGORIES"
              :key="cat"
              @click="activeCategory = cat"
              :class="[
                'px-6 py-2 rounded-full text-xs font-bold tracking-widest uppercase transition-all',
                activeCategory === cat
                  ? 'bg-emerald-950 text-white shadow-lg'
                  : 'bg-stone-100 text-emerald-950 hover:bg-stone-200'
              ]"
            >
              {{ CATEGORIES_MAP[cat] }}
            </button>
          </div>
        </div>

        <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-10">
          <ProductCard
            v-for="product in filteredProducts"
            :key="product.id"
            :product="product"
            @add-to-cart="addToCart"
          />
        </div>
      </section>
    </main>

    <!-- Footer 省略（你可以直接貼原本 JSX 的 HTML，改成 class） -->

    <!-- Cart Drawer -->
    <div v-if="isCartOpen" class="fixed inset-0 z-[110] flex justify-end">
      <div
        class="absolute inset-0 bg-stone-900/60 backdrop-blur-sm"
        @click="isCartOpen = false"
      />

      <div class="relative w-full max-w-md bg-white h-full shadow-2xl flex flex-col">
        <div class="p-6 border-b flex justify-between items-center">
          <h3 class="text-xl font-bold uppercase tracking-widest">
            您的裝備袋
          </h3>
          <button @click="isCartOpen = false">
            ✕
          </button>
        </div>

        <div class="flex-1 overflow-y-auto p-6">
          <div v-if="cart.length === 0" class="text-center text-stone-400">
            裝備袋是空的
          </div>

          <div v-else class="space-y-6">
            <div
              v-for="item in cart"
              :key="item.id"
              class="flex gap-4"
            >
              <img :src="item.image" class="w-20 h-20 object-cover rounded-lg" />

              <div class="flex-1">
                <div class="flex justify-between">
                  <h4 class="font-bold text-sm">{{ item.name }}</h4>
                  <button @click="removeFromCart(item.id)">
                    刪除
                  </button>
                </div>

                <p class="text-xs text-stone-500">
                  數量: {{ item.quantity }}
                </p>

                <p class="text-orange-700 font-bold text-sm">
                  ${{ (item.price * item.quantity).toFixed(2) }}
                </p>
              </div>
            </div>
          </div>
        </div>

        <div class="p-6 border-t bg-stone-50">
          <div class="flex justify-between mb-4">
            <span>小計</span>
            <span class="font-bold">${{ totalPrice.toFixed(2) }}</span>
          </div>

          <button
            :disabled="cart.length === 0"
            class="w-full bg-emerald-950 text-white py-4 rounded-full"
          >
            安全結帳
          </button>
        </div>
      </div>
    </div>

    <!-- AI Advisor -->
    <GeminiAdvisor
      :isOpen="isAdvisorOpen"
      @close="isAdvisorOpen = false"
      @add-to-cart="addToCart"
    />
  </div>
</template>

<script setup>
import { ref, computed } from "vue";
import Navbar from "./components/Navbar.vue";
import Hero from "./components/Hero.vue";
import ProductCard from "./components/ProductCard.vue";
import GeminiAdvisor from "./components/GeminiAdvisor.vue";
import { PRODUCTS, CATEGORIES, CATEGORIES_MAP } from "./constants";

// state
const cart = ref([]);
const activeCategory = ref("All");
const isAdvisorOpen = ref(false);
const isCartOpen = ref(false);

// computed
const filteredProducts = computed(() => {
  if (activeCategory.value === "All") return PRODUCTS;
  return PRODUCTS.filter(
    (p) => p.category === activeCategory.value
  );
});

const totalItems = computed(() =>
  cart.value.reduce((sum, item) => sum + item.quantity, 0)
);

const totalPrice = computed(() =>
  cart.value.reduce(
    (sum, item) => sum + item.price * item.quantity,
    0
  )
);

// methods
const addToCart = (product) => {
  const existing = cart.value.find(
    (item) => item.id === product.id
  );

  if (existing) {
    existing.quantity++;
  } else {
    cart.value.push({
      ...product,
      quantity: 1,
    });
  }
};

const removeFromCart = (id) => {
  cart.value = cart.value.filter(
    (item) => item.id !== id
  );
};
</script>
