<template>
  <div class="border rounded-xl overflow-hidden shadow hover:shadow-lg transition">
    <img :src="product.image" class="h-60 w-full object-cover" />

    <div class="p-6">
      <h3 class="font-bold text-lg mb-2">
        {{ product.name }}
      </h3>

      <p class="text-orange-600 font-bold mb-4">
        ${{ product.price }}
      </p>

      <button
        @click="$emit('add-to-cart', product)"
        class="w-full bg-emerald-950 text-white py-2 rounded-full"
      >
        加入裝備袋
      </button>
    </div>
  </div>
</template>

<script setup>
defineProps({
  product: Object
});

defineEmits(['add-to-cart']);
</script>
