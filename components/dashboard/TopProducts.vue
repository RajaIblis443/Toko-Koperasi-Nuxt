<template>
  <div class="bg-white rounded-xl shadow-sm overflow-hidden">
    <div class="px-6 py-4 border-b border-gray-100 flex justify-between items-center">
      <h2 class="font-semibold text-lg text-gray-800">Produk Terlaris</h2>
      <button class="text-sm text-primary-600 hover:text-primary-700 font-medium">
        Kelola Produk
      </button>
    </div>
    
    <div class="p-3">
      <div v-if="products.length === 0" class="p-4 text-center text-gray-500">
        Belum ada produk
      </div>
      
      <div v-for="(product, index) in products" :key="index" class="flex items-center py-3 px-4 border-b border-gray-100 last:border-0 hover:bg-gray-50 rounded-lg transition-colors">
        <div class="w-12 h-12 rounded-lg bg-gray-100 flex items-center justify-center overflow-hidden">
          <img v-if="product.image" :src="product.image" :alt="product.name" class="w-full h-full object-cover">
          <div v-else class="text-3xl text-gray-300">📦</div>
        </div>
        
        <div class="ml-4 flex-grow">
          <div class="flex justify-between">
            <p class="text-sm font-medium text-gray-900">{{ product.name }}</p>
            <p class="text-sm font-medium text-gray-900">{{ product.price }}</p>
          </div>
          <div class="flex justify-between mt-1">
            <p class="text-xs text-gray-500">Stok: {{ product.stock }}</p>
            <p class="text-xs text-gray-500">{{ product.sold }} terjual</p>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
defineProps({
  products: {
    type: Array,
    default: () => []
  }
});
</script>