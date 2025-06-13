/toko-koperasi/components/ui/Notifikasi.vue
<template>
  <div class="fixed bottom-4 right-4 z-50">
    <div 
      v-for="(item, index) in items" 
      :key="index" 
      :class="[
        'transition-all duration-300 transform translate-y-0 opacity-100 mb-3 p-4 rounded-md shadow-lg max-w-md flex items-center', 
        item.tipe === 'sukses' ? 'bg-green-50 border-l-4 border-green-500' : 'bg-red-50 border-l-4 border-red-500'
      ]"
    >
      <div :class="item.tipe === 'sukses' ? 'text-green-500' : 'text-red-500'">
        <svg v-if="item.tipe === 'sukses'" xmlns="http://www.w3.org/2000/svg" class="h-6 w-6" fill="none" viewBox="0 0 24 24" stroke="currentColor">
          <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 12l2 2 4-4m6 2a9 9 0 11-18 0 9 9 0 0118 0z" />
        </svg>
        <svg v-else xmlns="http://www.w3.org/2000/svg" class="h-6 w-6" fill="none" viewBox="0 0 24 24" stroke="currentColor">
          <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M10 14l2-2m0 0l2-2m-2 2l-2-2m2 2l2 2m7-2a9 9 0 11-18 0 9 9 0 0118 0z" />
        </svg>
      </div>
      <div class="ml-3">
        <p :class="item.tipe === 'sukses' ? 'text-green-800' : 'text-red-800'">{{ item.pesan }}</p>
      </div>
      <button @click="$emit('hapus', index)" class="ml-auto">
        <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5 text-gray-400" fill="none" viewBox="0 0 24 24" stroke="currentColor">
          <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12" />
        </svg>
      </button>
    </div>
  </div>
</template>

<script setup>
defineProps({
  items: Array
});

defineEmits(['hapus']);
</script>