<template>
  <div class="bg-white rounded-xl shadow-sm p-6">
    <div class="flex justify-between items-center mb-6">
      <div>
        <h2 class="text-lg font-semibold text-gray-800">Penjualan per Kategori</h2>
        <p class="text-sm text-gray-500 mt-1">Distribusi penjualan bulan ini</p>
      </div>
      <button class="p-2 text-sm rounded-lg text-gray-600 hover:bg-gray-100 transition-colors">
        <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5" fill="none" viewBox="0 0 24 24" stroke="currentColor">
          <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M5 12h.01M12 12h.01M19 12h.01M6 12a1 1 0 11-2 0 1 1 0 012 0zm7 0a1 1 0 11-2 0 1 1 0 012 0zm7 0a1 1 0 11-2 0 1 1 0 012 0z" />
        </svg>
      </button>
    </div>
    
    <div class="flex flex-col sm:flex-row">
      <div class="w-full sm:w-1/2 h-[240px] flex items-center justify-center">
        <Doughnut
          v-if="chartData"
          :data="chartData"
          :options="chartOptions"
        />
      </div>
      
      <div class="w-full sm:w-1/2 pl-0 sm:pl-4 pt-4 sm:pt-0">
        <div class="space-y-4">
          <div v-for="(item, index) in legendItems" :key="index" class="flex items-center">
            <div class="w-3 h-3 rounded-full mr-3" :style="{ backgroundColor: item.color }"></div>
            <div class="flex-grow">
              <div class="flex justify-between">
                <span class="text-sm font-medium text-gray-800">{{ item.label }}</span>
                <span class="text-sm font-medium text-gray-800">{{ item.value }}</span>
              </div>
              <div class="w-full bg-gray-100 rounded-full h-1.5 mt-1">
                <div class="h-1.5 rounded-full" :style="{ width: item.percentage, backgroundColor: item.color }"></div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue';
import { Doughnut } from 'vue-chartjs';
import { Chart as ChartJS, ArcElement, Tooltip, Legend } from 'chart.js';

// Register Chart.js components
ChartJS.register(ArcElement, Tooltip, Legend);

const chartData = ref(null);
const chartOptions = ref({
  responsive: true,
  maintainAspectRatio: false,
  cutout: '70%',
  plugins: {
    legend: {
      display: false
    },
    tooltip: {
      backgroundColor: 'rgba(255, 255, 255, 0.9)',
      titleColor: '#333',
      bodyColor: '#666',
      borderColor: 'rgba(0, 0, 0, 0.1)',
      borderWidth: 1,
      padding: 10,
      boxPadding: 5,
      usePointStyle: true
    }
  }
});

// Chart legend items with percentages
const legendItems = ref([
  { label: 'Sembako', value: 'Rp 5.480.000', percentage: '45%', color: '#4CAF50' },
  { label: 'Makanan & Minuman', value: 'Rp 3.250.000', percentage: '27%', color: '#2196F3' },
  { label: 'Kebersihan', value: 'Rp 1.850.000', percentage: '15%', color: '#FFC107' },
  { label: 'Alat Tulis', value: 'Rp 1.200.000', percentage: '10%', color: '#9C27B0' },
  { label: 'Lain-lain', value: 'Rp 380.000', percentage: '3%', color: '#607D8B' }
]);

onMounted(() => {
  // Simulate data loading
  setTimeout(() => {
    chartData.value = {
      labels: legendItems.value.map(item => item.label),
      datasets: [
        {
          data: [45, 27, 15, 10, 3],
          backgroundColor: legendItems.value.map(item => item.color),
          borderWidth: 0
        }
      ]
    };
  }, 200);
});
</script>