<template>
  <div v-if="isOpen" class="fixed inset-0 flex items-center justify-center bg-black/50 z-50">
    <div class="bg-white p-8 rounded-xl w-96 relative">
      <button
        @click="$emit('close')"
        class="absolute top-4 right-4"
      >
        ✕
      </button>

      <h3 class="text-xl font-bold mb-4">
        AI 裝備顧問
      </h3>

      <p class="text-sm text-gray-600 mb-6">
        請輸入你的登山需求，AI 會幫你推薦裝備。
      </p>

      <button
        class="w-full bg-emerald-950 text-white py-2 rounded-full"
        @click="mockAdd"
      >
        推薦一件裝備
      </button>
    </div>
  </div>
</template>

<script setup>
import { PRODUCTS } from '../constants';

defineProps({
  isOpen: Boolean
});

const emit = defineEmits(['close', 'add-to-cart']);

const mockAdd = () => {
  emit('add-to-cart', PRODUCTS[0]);
  emit('close');
};
</script>
