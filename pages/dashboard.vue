<script setup>
definePageMeta({
  layout: 'default'
})

// Data untuk StatCard
const stats = ref([
  { 
    title: 'Pendapatan Hari Ini', 
    value: 'Rp 2.500.000', 
    trend: '12.5% dari kemarin',
    trendingUp: true,
    iconPath: 'M12 8c-1.657 0-3 .895-3 2s1.343 2 3 2 3 .895 3 2-1.343 2-3 2m0-8c1.11 0 2.08.402 2.599 1M12 8V7m0 1v8m0 0v1m0-1c-1.11 0-2.08-.402-2.599-1M21 12a9 9 0 11-18 0 9 9 0 0118 0z',
    iconBgClass: 'bg-primary-100',
    iconColor: 'text-primary-600'
  },
  { 
    title: 'Total Produk', 
    value: '125', 
    trend: '5 produk baru',
    trendingUp: true,
    iconPath: 'M16 11V7a4 4 0 00-8 0v4M5 9h14l1 12H4L5 9z',
    iconBgClass: 'bg-blue-100',
    iconColor: 'text-blue-600'
  },
  { 
    title: 'Total Anggota', 
    value: '64', 
    trend: '3 anggota baru',
    trendingUp: true,
    iconPath: 'M17 20h5v-2a3 3 0 00-5.356-1.857M17 20H7m10 0v-2c0-.656-.126-1.283-.356-1.857M7 20H2v-2a3 3 0 015.356-1.857M7 20v-2c0-.656.126-1.283.356-1.857m0 0a5.002 5.002 0 019.288 0M15 7a3 3 0 11-6 0 3 3 0 016 0zm6 3a2 2 0 11-4 0 2 2 0 014 0zM7 10a2 2 0 11-4 0 2 2 0 014 0z',
    iconBgClass: 'bg-orange-100',
    iconColor: 'text-orange-600'
  },
  { 
    title: 'Total Transaksi', 
    value: '243', 
    trend: '12 transaksi baru',
    trendingUp: true,
    iconPath: 'M9 12l2 2 4-4m6 2a9 9 0 11-18 0 9 9 0 0118 0z',
    iconBgClass: 'bg-red-100',
    iconColor: 'text-red-600'
  }
]);

// Data untuk transaksi terbaru
const recentTransactions = ref([
  { 
    description: 'Pembelian Beras', 
    amount: 'Rp 250.000', 
    customer: 'Ahmad Fauzi', 
    date: '10 menit yang lalu',
    status: 'success'
  },
  { 
    description: 'Pembayaran Listrik', 
    amount: '-Rp 175.000', 
    customer: 'Koperasi', 
    date: '30 menit yang lalu',
    status: 'success'
  },
  { 
    description: 'Pembelian Minyak Goreng', 
    amount: 'Rp 120.000', 
    customer: 'Siti Aminah', 
    date: '1 jam yang lalu',
    status: 'pending'
  },
  { 
    description: 'Pembelian Gagal', 
    amount: 'Rp 0', 
    customer: 'Budi Santoso', 
    date: '2 jam yang lalu',
    status: 'failed'
  },
  { 
    description: 'Pembelian Sembako', 
    amount: 'Rp 350.000', 
    customer: 'Dewi Lestari', 
    date: '3 jam yang lalu',
    status: 'success'
  }
]);

// Data untuk produk terlaris
const topProducts = ref([
  {
    name: 'Beras Premium 5kg',
    price: 'Rp 68.000',
    stock: 45,
    sold: 120,
    image: '/img/rice.jpg'
  },
  {
    name: 'Minyak Goreng 1L',
    price: 'Rp 24.000',
    stock: 32,
    sold: 98,
    image: '/img/oil.jpg'
  },
  {
    name: 'Gula Pasir 1kg',
    price: 'Rp 16.500',
    stock: 56,
    sold: 87,
    image: '/img/sugar.jpg'
  },
  {
    name: 'Tepung Terigu 1kg',
    price: 'Rp 12.000',
    stock: 28,
    sold: 65,
    image: ''
  }
]);
</script>

<template>
  <div>
    <h1 class="text-2xl font-bold mb-6">Dashboard Koperasi</h1>
    
    <!-- Stats Cards -->
    <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-6 mb-6">
      <DashboardStatCard 
        v-for="(stat, index) in stats" 
        :key="index"
        :title="stat.title"
        :value="stat.value"
        :trend="stat.trend"
        :trending-up="stat.trendingUp"
        :icon-path="stat.iconPath"
        :icon-bg-class="stat.iconBgClass"
        :icon-color="stat.iconColor"
      />
    </div>
    
    <!-- Main Content Area -->
    <div class="grid grid-cols-1 lg:grid-cols-3 gap-6 mb-6">
      <!-- Recent Transactions -->
      <div class="lg:col-span-2">
        <DashboardRecentTransactions :transactions="recentTransactions" />
      </div>
      
      <!-- Top Products -->
      <div>
        <DashboardTopProducts :products="topProducts" />
      </div>
    </div>
    
    <!-- Additional Content -->
    <div class="bg-white rounded-xl shadow-sm p-6">
      <h2 class="text-lg font-semibold mb-4">Statistik Penjualan</h2>
      <p class="text-gray-600">
        Grafik statistik penjualan dapat ditampilkan di sini menggunakan library seperti Chart.js, ApexCharts, dll.
      </p>
      <div class="w-full h-64 bg-gray-50 rounded-lg border border-dashed border-gray-300 flex items-center justify-center mt-4">
        <span class="text-gray-400">Widget Grafik</span>
      </div>
    </div>
  </div>
</template>