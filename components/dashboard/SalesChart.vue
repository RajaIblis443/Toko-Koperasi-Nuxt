<template>
  <div class="bg-white rounded-xl shadow-sm p-6">
    <div class="flex justify-between items-center mb-6">
      <div>
        <h2 class="text-lg font-semibold text-gray-800">Statistik Penjualan</h2>
        <p class="text-sm text-gray-500 mt-1">{{ filterTitle }}</p>
      </div>
      <div class="flex space-x-2">
        <button 
          @click="changeFilter('week')" 
          class="px-3 py-1 text-sm rounded-lg transition-colors"
          :class="activeFilter === 'week' ? 'bg-primary-50 text-primary-600' : 'text-gray-600 hover:bg-gray-100'"
        >
          Minggu Ini
        </button>
        <button 
          @click="changeFilter('month')" 
          class="px-3 py-1 text-sm rounded-lg transition-colors"
          :class="activeFilter === 'month' ? 'bg-primary-50 text-primary-600' : 'text-gray-600 hover:bg-gray-100'"
        >
          Bulan Ini
        </button>
        <button 
          @click="changeFilter('year')" 
          class="px-3 py-1 text-sm rounded-lg transition-colors" 
          :class="activeFilter === 'year' ? 'bg-primary-50 text-primary-600' : 'text-gray-600 hover:bg-gray-100'"
        >
          Tahun Ini
        </button>
      </div>
    </div>
    
    <!-- Chart Area with Loading Indicator -->
    <div class="h-[320px] relative">
      <div v-if="!chartData" class="absolute inset-0 flex items-center justify-center">
        <div class="flex flex-col items-center">
          <div class="w-12 h-12 border-4 border-t-primary-500 border-primary-200 rounded-full animate-spin"></div>
          <span class="mt-3 text-gray-500 text-sm">Memuat data...</span>
        </div>
      </div>
      <LineChart
        v-else
        :data="chartData"
        :options="chartOptions"
      />
    </div>
    
    <!-- Summary Stats -->
    <div class="grid grid-cols-2 sm:grid-cols-4 gap-4 mt-6 pt-6 border-t border-gray-100">
      <div class="text-center">
        <p class="text-sm text-gray-500 mb-1">Total Penjualan</p>
        <p class="text-xl font-bold text-gray-800">{{ summaryStats.total }}</p>
        <p class="text-xs text-green-500 flex items-center justify-center mt-1">
          <svg class="w-3 h-3 mr-1" fill="currentColor" viewBox="0 0 20 20">
            <path fill-rule="evenodd" d="M12 7a1 1 0 110-2h5a1 1 0 011 1v5a1 1 0 11-2 0V8.414l-4.293 4.293a1 1 0 01-1.414 0L8 10.414l-4.293 4.293a1 1 0 01-1.414-1.414l5-5a1 1 0 011.414 0L11 10.586 14.586 7H12z" clip-rule="evenodd"></path>
          </svg>
          {{ summaryStats.totalTrend }}
        </p>
      </div>
      
      <div class="text-center">
        <p class="text-sm text-gray-500 mb-1">Transaksi</p>
        <p class="text-xl font-bold text-gray-800">{{ summaryStats.transactions }}</p>
        <p class="text-xs text-green-500 flex items-center justify-center mt-1">
          <svg class="w-3 h-3 mr-1" fill="currentColor" viewBox="0 0 20 20">
            <path fill-rule="evenodd" d="M12 7a1 1 0 110-2h5a1 1 0 011 1v5a1 1 0 11-2 0V8.414l-4.293 4.293a1 1 0 01-1.414 0L8 10.414l-4.293 4.293a1 1 0 01-1.414-1.414l5-5a1 1 0 011.414 0L11 10.586 14.586 7H12z" clip-rule="evenodd"></path>
          </svg>
          {{ summaryStats.transactionsTrend }}
        </p>
      </div>
      
      <div class="text-center">
        <p class="text-sm text-gray-500 mb-1">Rata-rata</p>
        <p class="text-xl font-bold text-gray-800">{{ summaryStats.average }}</p>
        <p class="text-xs text-green-500 flex items-center justify-center mt-1">
          <svg class="w-3 h-3 mr-1" fill="currentColor" viewBox="0 0 20 20">
            <path fill-rule="evenodd" d="M12 7a1 1 0 110-2h5a1 1 0 011 1v5a1 1 0 11-2 0V8.414l-4.293 4.293a1 1 0 01-1.414 0L8 10.414l-4.293 4.293a1 1 0 01-1.414-1.414l5-5a1 1 0 011.414 0L11 10.586 14.586 7H12z" clip-rule="evenodd"></path>
          </svg>
          {{ summaryStats.averageTrend }}
        </p>
      </div>
      
      <div class="text-center">
        <p class="text-sm text-gray-500 mb-1">Produk Terjual</p>
        <p class="text-xl font-bold text-gray-800">{{ summaryStats.products }}</p>
        <p class="text-xs text-green-500 flex items-center justify-center mt-1">
          <svg class="w-3 h-3 mr-1" fill="currentColor" viewBox="0 0 20 20">
            <path fill-rule="evenodd" d="M12 7a1 1 0 110-2h5a1 1 0 011 1v5a1 1 0 11-2 0V8.414l-4.293 4.293a1 1 0 01-1.414 0L8 10.414l-4.293 4.293a1 1 0 01-1.414-1.414l5-5a1 1 0 011.414 0L11 10.586 14.586 7H12z" clip-rule="evenodd"></path>
          </svg>
          {{ summaryStats.productsTrend }}
        </p>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed, onMounted } from 'vue';
import { Line as LineChart } from 'vue-chartjs';
import { Chart as ChartJS, CategoryScale, LinearScale, PointElement, LineElement, Title, Tooltip, Legend, Filler } from 'chart.js';

// Register Chart.js components
ChartJS.register(CategoryScale, LinearScale, PointElement, LineElement, Title, Tooltip, Legend, Filler);

// State untuk filter aktif
const activeFilter = ref('week'); // 'week', 'month', 'year'

// Judul dinamis untuk filter
const filterTitle = computed(() => {
  if (activeFilter.value === 'week') return 'Data penjualan 7 hari terakhir';
  if (activeFilter.value === 'month') return 'Data penjualan bulan ini';
  return 'Data penjualan tahun 2025';
});

// Summary stats berdasarkan periode waktu
const summaryStats = computed(() => {
  if (activeFilter.value === 'week') {
    return {
      total: 'Rp 12.580.000',
      totalTrend: '8.2% dibanding minggu lalu',
      transactions: '243',
      transactionsTrend: '5.3% dibanding minggu lalu',
      average: 'Rp 51.770',
      averageTrend: '2.8% dibanding minggu lalu',
      products: '1,254',
      productsTrend: '10.5% dibanding minggu lalu'
    };
  } else if (activeFilter.value === 'month') {
    return {
      total: 'Rp 46.200.000',
      totalTrend: '12.5% dibanding bulan lalu',
      transactions: '980',
      transactionsTrend: '7.8% dibanding bulan lalu',
      average: 'Rp 47.140',
      averageTrend: '4.3% dibanding bulan lalu',
      products: '4,850',
      productsTrend: '15.2% dibanding bulan lalu'
    };
  } else {
    return {
      total: 'Rp 451.000.000',
      totalTrend: '24.3% dibanding tahun lalu',
      transactions: '10,432',
      transactionsTrend: '18.7% dibanding tahun lalu',
      average: 'Rp 43.230',
      averageTrend: '5.1% dibanding tahun lalu',
      products: '52,876',
      productsTrend: '22.4% dibanding tahun lalu'
    };
  }
});

// Chart data
const chartData = ref(null);
const chartOptions = ref({
  responsive: true,
  maintainAspectRatio: false,
  elements: {
    line: {
      tension: 0.4
    },
    point: {
      radius: 0,
      hitRadius: 10,
      hoverRadius: 5
    }
  },
  scales: {
    y: {
      beginAtZero: true,
      grid: {
        display: true,
        color: 'rgba(0, 0, 0, 0.05)'
      },
      ticks: {
        callback: function(value) {
          return 'Rp ' + formatNumber(value);
        }
      }
    },
    x: {
      grid: {
        display: false
      }
    }
  },
  plugins: {
    legend: {
      position: 'top',
      align: 'end',
      labels: {
        boxWidth: 10,
        usePointStyle: true,
        pointStyle: 'circle'
      }
    },
    tooltip: {
      backgroundColor: 'rgba(255, 255, 255, 0.9)',
      titleColor: '#333',
      bodyColor: '#666',
      borderColor: 'rgba(0, 0, 0, 0.1)',
      borderWidth: 1,
      padding: 10,
      boxPadding: 5,
      usePointStyle: true,
      callbacks: {
        label: function(context) {
          let label = context.dataset.label || '';
          if (label) {
            label += ': ';
          }
          if (context.parsed.y !== null) {
            label += 'Rp ' + formatNumber(context.parsed.y);
          }
          return label;
        }
      }
    }
  }
});

// Fungsi format angka dengan titik
const formatNumber = (num) => {
  return num.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ".");
};

// Get last 7 days for labels
const getLastSevenDays = () => {
  const days = [];
  const dayNames = ['Minggu', 'Senin', 'Selasa', 'Rabu', 'Kamis', 'Jumat', 'Sabtu'];
  
  for (let i = 6; i >= 0; i--) {
    const date = new Date();
    date.setDate(date.getDate() - i);
    days.push(dayNames[date.getDay()]);
  }
  
  return days;
};

// Fungsi untuk mengubah filter
const changeFilter = (filter) => {
  activeFilter.value = filter;
  updateChartData(filter);
};

// Data untuk filter mingguan
const getWeeklyData = () => {
  return {
    labels: getLastSevenDays(),
    datasets: [
      {
        label: 'Penjualan',
        backgroundColor: 'rgba(76, 175, 80, 0.1)',
        borderColor: '#4CAF50',
        data: [1250000, 1800000, 1600000, 2100000, 1700000, 1900000, 2230000],
        fill: true
      },
      {
        label: 'Target',
        backgroundColor: 'rgba(66, 133, 244, 0)',
        borderColor: '#4285F4',
        borderDash: [5, 5],
        data: [1500000, 1500000, 1500000, 1500000, 1500000, 1500000, 1500000],
        fill: false
      }
    ]
  };
};

// Data untuk filter bulanan
const getMonthlyData = () => {
  return {
    labels: ['Minggu 1', 'Minggu 2', 'Minggu 3', 'Minggu 4', 'Minggu 5'],
    datasets: [
      {
        label: 'Penjualan',
        backgroundColor: 'rgba(76, 175, 80, 0.1)',
        borderColor: '#4CAF50',
        data: [9500000, 10200000, 8700000, 11500000, 6300000],
        fill: true
      },
      {
        label: 'Target',
        backgroundColor: 'rgba(66, 133, 244, 0)',
        borderColor: '#4285F4',
        borderDash: [5, 5],
        data: [9000000, 9000000, 9000000, 9000000, 9000000],
        fill: false
      }
    ]
  };
};

// Data untuk filter tahunan
const getYearlyData = () => {
  return {
    labels: ['Jan', 'Feb', 'Mar', 'Apr', 'Mei', 'Jun', 'Jul', 'Agt', 'Sep', 'Okt', 'Nov', 'Des'],
    datasets: [
      {
        label: 'Penjualan',
        backgroundColor: 'rgba(76, 175, 80, 0.1)',
        borderColor: '#4CAF50',
        data: [35000000, 32000000, 38000000, 30000000, 39000000, 42000000, 41000000, 35000000, 44000000, 40000000, 45000000, 50000000],
        fill: true
      },
      {
        label: 'Target',
        backgroundColor: 'rgba(66, 133, 244, 0)',
        borderColor: '#4285F4',
        borderDash: [5, 5],
        data: [36000000, 36000000, 36000000, 36000000, 36000000, 36000000, 36000000, 36000000, 36000000, 36000000, 36000000, 36000000],
        fill: false
      }
    ]
  };
};

// Fungsi untuk memperbarui data grafik berdasarkan filter yang dipilih
const updateChartData = (filter) => {
  // Reset data untuk menampilkan loading state
  chartData.value = null;
  
  // Simulasi loading, bisa diganti dengan API call
  setTimeout(() => {
    if (filter === 'week') {
      chartData.value = getWeeklyData();
    } else if (filter === 'month') {
      chartData.value = getMonthlyData();
    } else {
      chartData.value = getYearlyData();
    }
  }, 500);
};

onMounted(() => {
  // Inisialisasi dengan data sesuai filter default
  updateChartData(activeFilter.value);
});
</script>