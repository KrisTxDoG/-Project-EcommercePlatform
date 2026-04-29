<!-- components/Navbar.vue -->
<template>
  <nav class="bg-emerald-950 text-white p-4 flex justify-between">
    <span class="font-bold text-lg">LOGO</span>
    <button @click="$emit('open-cart')">Cart ({{ cartCount }})</button>
    <button @click="$emit('open-advisor')">Advisor</button>
  </nav>
</template>

<script setup>
defineProps({
  cartCount: Number
})
defineEmits(['open-cart', 'open-advisor'])
</script>
