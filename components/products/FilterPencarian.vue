/rifaicandra/development/toko-koperasi/components/products/FilterPencarian.vue
<template>
  <div class="bg-white p-4 rounded-xl shadow-sm flex flex-col md:flex-row justify-between mb-6">
    <div class="flex flex-col sm:flex-row items-start sm:items-center space-y-2 sm:space-y-0 sm:space-x-4 mb-4 md:mb-0">
      <div class="w-full sm:w-64">
        <div class="relative">
          <input 
            :value="kataPencarian" 
            @input="$emit('update:kataPencarian', $event.target.value)" 
            type="text" 
            placeholder="Cari produk..." 
            class="w-full pl-10 pr-4 py-2 border border-gray-300 rounded-lg focus:ring-primary-500 focus:border-primary-500"
          />
          <div class="absolute left-0 inset-y-0 flex items-center pl-3 pointer-events-none">
            <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5 text-gray-400" fill="none" viewBox="0 0 24 24" stroke="currentColor">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M21 21l-6-6m2-5a7 7 0 11-14 0 7 7 0 0114 0z" />
            </svg>
          </div>
        </div>
      </div>
    </div>
    
    <div class="flex items-center space-x-4">
      <select 
        :value="urutkan" 
        @change="$emit('update:urutkan', $event.target.value)" 
        class="py-2 px-3 border border-gray-300 rounded-lg focus:ring-primary-500 focus:border-primary-500"
      >
        <option value="name_produk-asc">Nama (A-Z)</option>
        <option value="name_produk-desc">Nama (Z-A)</option>
        <option value="harga_jual-asc">Harga Jual (Rendah-Tinggi)</option>
        <option value="harga_jual-desc">Harga Jual (Tinggi-Rendah)</option>
        <option value="stok-asc">Stok (Rendah-Tinggi)</option>
        <option value="stok-desc">Stok (Tinggi-Rendah)</option>
      </select>
    </div>
  </div>
</template>

<script setup>
defineProps({
  kataPencarian: String,
  urutkan: String
});

defineEmits(['update:kataPencarian', 'update:urutkan']);
</script>