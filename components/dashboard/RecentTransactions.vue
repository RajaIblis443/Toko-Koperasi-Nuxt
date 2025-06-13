<template>
  <div class="bg-white rounded-xl shadow-sm overflow-hidden">
    <div class="px-6 py-4 border-b border-gray-100 flex justify-between items-center">
      <h2 class="font-semibold text-lg text-gray-800">Transaksi Terbaru</h2>
      <button class="text-sm text-primary-600 hover:text-primary-700 font-medium">
        Lihat Semua
      </button>
    </div>
    
    <div class="p-3">
      <div v-if="transactions.length === 0" class="p-4 text-center text-gray-500">
        Belum ada transaksi
      </div>
      
      <div v-for="(transaction, index) in transactions" :key="index" class="flex items-center py-3 px-4 border-b border-gray-100 last:border-0 hover:bg-gray-50 rounded-lg transition-colors">
        <div :class="`w-10 h-10 rounded-lg text-white flex items-center justify-center ${getStatusColor(transaction.status)}`">
          <svg v-if="transaction.status === 'success'" xmlns="http://www.w3.org/2000/svg" class="h-5 w-5" fill="none" viewBox="0 0 24 24" stroke="currentColor">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M5 13l4 4L19 7" />
          </svg>
          <svg v-else-if="transaction.status === 'pending'" xmlns="http://www.w3.org/2000/svg" class="h-5 w-5" fill="none" viewBox="0 0 24 24" stroke="currentColor">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 8v4l3 3m6-3a9 9 0 11-18 0 9 9 0 0118 0z" />
          </svg>
          <svg v-else xmlns="http://www.w3.org/2000/svg" class="h-5 w-5" fill="none" viewBox="0 0 24 24" stroke="currentColor">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12" />
          </svg>
        </div>
        
        <div class="ml-4 flex-grow">
          <div class="flex justify-between">
            <p class="text-sm font-medium text-gray-900">{{ transaction.description }}</p>
            <p :class="`text-sm font-semibold ${transaction.amount.startsWith('-') ? 'text-red-600' : 'text-green-600'}`">{{ transaction.amount }}</p>
          </div>
          <div class="flex justify-between mt-1">
            <p class="text-xs text-gray-500">{{ transaction.customer }}</p>
            <p class="text-xs text-gray-500">{{ transaction.date }}</p>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
defineProps({
  transactions: {
    type: Array,
    default: () => []
  }
});

function getStatusColor(status) {
  switch(status) {
    case 'success': return 'bg-green-500';
    case 'pending': return 'bg-yellow-500';
    case 'failed': return 'bg-red-500';
    default: return 'bg-gray-500';
  }
}
</script>