<template>
  <div v-if="show" class="fixed z-50 inset-0 overflow-y-auto">
    <div class="flex items-center justify-center min-h-screen pt-4 px-4 pb-20 text-center sm:block sm:p-0">
      <div class="fixed inset-0 bg-gray-500 bg-opacity-75 transition-opacity" @click="$emit('tutup')"></div>
      
      <div class="inline-block align-bottom bg-white rounded-lg text-left overflow-hidden shadow-xl transform transition-all sm:my-8 sm:align-middle sm:max-w-lg sm:w-full">
        <div class="bg-white px-4 pt-5 pb-4 sm:p-6 sm:pb-4">
          <h3 class="text-lg leading-6 font-medium text-gray-900">{{ modeEdit ? 'Edit Produk' : 'Tambah Produk Baru' }}</h3>
          <div class="mt-4">
            <form @submit.prevent="$emit('simpan', formData)">
              <div class="mb-4">
                <label class="block text-sm font-medium text-gray-700">Nama Produk</label>
                <input v-model="formData.name_produk" type="text" class="mt-1 focus:ring-primary-500 focus:border-primary-500 block w-full shadow-sm sm:text-sm border-gray-300 rounded-md" required>
              </div>
              
              <div class="mb-4">
                <label class="block text-sm font-medium text-gray-700">Kode Produk</label>
                <input v-model="formData.kode_produk" type="text" class="mt-1 focus:ring-primary-500 focus:border-primary-500 block w-full shadow-sm sm:text-sm border-gray-300 rounded-md" required>
              </div>
              
              <div class="grid grid-cols-2 gap-4 mb-4">
                <div>
                  <label class="block text-sm font-medium text-gray-700">Harga Beli (Rp)</label>
                  <input v-model.number="formData.harga_beli" type="number" min="0" step="0.01" class="mt-1 focus:ring-primary-500 focus:border-primary-500 block w-full shadow-sm sm:text-sm border-gray-300 rounded-md" required>
                </div>
                
                <div>
                  <label class="block text-sm font-medium text-gray-700">Harga Jual (Rp)</label>
                  <input v-model.number="formData.harga_jual" type="number" min="0" step="0.01" class="mt-1 focus:ring-primary-500 focus:border-primary-500 block w-full shadow-sm sm:text-sm border-gray-300 rounded-md" required>
                </div>
              </div>
              
              <div class="grid grid-cols-2 gap-4 mb-4">
                <div>
                  <label class="block text-sm font-medium text-gray-700">Stok</label>
                  <input v-model.number="formData.stok" type="number" min="0" class="mt-1 focus:ring-primary-500 focus:border-primary-500 block w-full shadow-sm sm:text-sm border-gray-300 rounded-md" required>
                </div>
                
                <div>
                  <label class="block text-sm font-medium text-gray-700">Tanggal Masuk</label>
                  <input v-model="formData.tanggal_masuk" type="date" class="mt-1 focus:ring-primary-500 focus:border-primary-500 block w-full shadow-sm sm:text-sm border-gray-300 rounded-md" required>
                </div>
              </div>
              
              <div class="bg-gray-50 px-4 py-3 sm:px-6 sm:flex sm:flex-row-reverse">
                <button type="submit" class="w-full inline-flex justify-center rounded-md border border-transparent shadow-sm px-4 py-2 bg-primary-600 text-base font-medium text-white hover:bg-primary-700 focus:outline-none sm:ml-3 sm:w-auto sm:text-sm">
                  {{ modeEdit ? 'Perbarui' : 'Simpan' }}
                </button>
                <button type="button" @click="$emit('tutup')" class="mt-3 w-full inline-flex justify-center rounded-md border border-gray-300 shadow-sm px-4 py-2 bg-white text-base font-medium text-gray-700 hover:bg-gray-50 focus:outline-none sm:mt-0 sm:w-auto sm:text-sm">
                  Batal
                </button>
              </div>
            </form>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, watch } from 'vue';

const props = defineProps({
  show: Boolean,
  modeEdit: Boolean,
  produk: Object
});

const emit = defineEmits(['tutup', 'simpan']);

const formData = ref({
  id_produk: null,
  kode_produk: '',
  name_produk: '',
  harga_beli: 0,
  harga_jual: 0,
  stok: 0,
  tanggal_masuk: new Date().toISOString().split('T')[0]
});

// Memantau perubahan produk untuk mengisi form
watch(() => props.produk, (newProduk) => {
  if (newProduk) {
    formData.value = { 
      ...newProduk,
      tanggal_masuk: newProduk.tanggal_masuk ? new Date(newProduk.tanggal_masuk).toISOString().split('T')[0] : new Date().toISOString().split('T')[0]
    };
  }
}, { immediate: true });
</script>