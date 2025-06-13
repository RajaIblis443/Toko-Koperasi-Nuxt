<template>
  <div class="min-h-screen bg-neutral-100 flex flex-col md:flex-row">
    <!-- Mobile Header -->
    <div class="md:hidden bg-white border-b border-gray-200 p-4 flex items-center justify-between sticky top-0 z-10">
      <div class="flex items-center">
        <button @click="toggleSidebar" class="p-2 mr-2">
          <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6 text-gray-600" fill="none" viewBox="0 0 24 24" stroke="currentColor">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 6h16M4 12h16M4 18h16" />
          </svg>
        </button>
        <span class="text-lg font-semibold text-gray-800">Koperasi App</span>
      </div>
      <div>
        <!-- Avatar/User Menu for Mobile -->
        <button class="w-8 h-8 rounded-full bg-primary-500 text-white flex items-center justify-center">
          <span>RC</span>
        </button>
      </div>
    </div>

    <!-- Sidebar -->
    <div :class="`${sidebarOpen ? 'translate-x-0' : '-translate-x-full'} md:translate-x-0 fixed md:relative z-20 transition-transform duration-300 ease-in-out h-full bg-white border-r border-gray-200 shadow-lg md:shadow-none w-64 flex-shrink-0`">
      <!-- Logo and Close Button -->
      <div class="flex items-center justify-between p-4 border-b border-gray-200">
        <div class="flex items-center">
          <svg xmlns="http://www.w3.org/2000/svg" class="h-8 w-8 text-primary-500 mr-2" fill="none" viewBox="0 0 24 24" stroke="currentColor">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="1.5" d="M13.5 21v-7.5a.75.75 0 01.75-.75h3a.75.75 0 01.75.75V21m-4.5 0H2.36m11.14 0H18m0 0h3.64m-1.39 0V9.349m-16.5 11.65V9.35m0 0a3.001 3.001 0 003.75-.615A2.993 2.993 0 009.75 9.75c.896 0 1.7-.393 2.25-1.016a2.993 2.993 0 002.25 1.016c.896 0 1.7-.393 2.25-1.016a3.001 3.001 0 003.75.614m-16.5 0a3.004 3.004 0 01-.621-4.72L4.318 3.44A1.5 1.5 0 015.378 3h13.243a1.5 1.5 0 011.06.44l1.19 1.189a3 3 0 01-.621 4.72m-13.5 8.65h3.75a.75.75 0 00.75-.75V13.5a.75.75 0 00-.75-.75H6.75a.75.75 0 00-.75.75v3.75c0 .415.336.75.75.75z" />
          </svg>
          <span class="text-lg font-semibold text-gray-800">Koperasi</span>
        </div>
        <!-- Close button (mobile only) -->
        <button @click="toggleSidebar" class="md:hidden p-2">
          <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6 text-gray-600" fill="none" viewBox="0 0 24 24" stroke="currentColor">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12" />
          </svg>
        </button>
      </div>
      
      <!-- Navigation Menu -->
      <nav class="py-4">
        <div class="px-4 mb-2 text-xs font-semibold text-gray-500 uppercase tracking-wider">Menu Utama</div>
        <NuxtLink to="/dashboard" class="py-2.5 px-4 rounded-lg mx-2 mb-1 flex items-center transition-colors hover:bg-neutral-100" :class="{ 'bg-primary-50 text-primary-700': $route.path === '/dashboard' }">
          <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5 mr-3" fill="none" viewBox="0 0 24 24" stroke="currentColor">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M3 12l2-2m0 0l7-7 7 7M5 10v10a1 1 0 001 1h3m10-11l2 2m-2-2v10a1 1 0 01-1 1h-3m-6 0a1 1 0 001-1v-4a1 1 0 011-1h2a1 1 0 011 1v4a1 1 0 001 1m-6 0h6" />
          </svg>
          <span>Dashboard</span>
        </NuxtLink>
        
        <NuxtLink to="/products" class="py-2.5 px-4 rounded-lg mx-2 mb-1 flex items-center transition-colors hover:bg-neutral-100" :class="{ 'bg-primary-50 text-primary-700': $route.path === '/products' }">
          <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5 mr-3" fill="none" viewBox="0 0 24 24" stroke="currentColor">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M16 11V7a4 4 0 00-8 0v4M5 9h14l1 12H4L5 9z" />
          </svg>
          <span>Produk</span>
        </NuxtLink>
        
        <NuxtLink to="/transactions" class="py-2.5 px-4 rounded-lg mx-2 mb-1 flex items-center transition-colors hover:bg-neutral-100" :class="{ 'bg-primary-50 text-primary-700': $route.path === '/transactions' }">
          <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5 mr-3" fill="none" viewBox="0 0 24 24" stroke="currentColor">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 5H7a2 2 0 00-2 2v12a2 2 0 002 2h10a2 2 0 002-2V7a2 2 0 00-2-2h-2M9 5a2 2 0 002 2h2a2 2 0 002-2M9 5a2 2 0 012-2h2a2 2 0 012 2" />
          </svg>
          <span>Transaksi</span>
        </NuxtLink>
        
        <NuxtLink to="/members" class="py-2.5 px-4 rounded-lg mx-2 mb-1 flex items-center transition-colors hover:bg-neutral-100" :class="{ 'bg-primary-50 text-primary-700': $route.path === '/members' }">
          <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5 mr-3" fill="none" viewBox="0 0 24 24" stroke="currentColor">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M17 20h5v-2a3 3 0 00-5.356-1.857M17 20H7m10 0v-2c0-.656-.126-1.283-.356-1.857M7 20H2v-2a3 3 0 015.356-1.857M7 20v-2c0-.656.126-1.283.356-1.857m0 0a5.002 5.002 0 019.288 0M15 7a3 3 0 11-6 0 3 3 0 016 0zm6 3a2 2 0 11-4 0 2 2 0 014 0zM7 10a2 2 0 11-4 0 2 2 0 014 0z" />
          </svg>
          <span>Anggota</span>
        </NuxtLink>
        
        <div class="mt-4 px-4 mb-2 text-xs font-semibold text-gray-500 uppercase tracking-wider">Laporan</div>
        
        <NuxtLink to="/reports/sales" class="py-2.5 px-4 rounded-lg mx-2 mb-1 flex items-center transition-colors hover:bg-neutral-100" :class="{ 'bg-primary-50 text-primary-700': $route.path === '/reports/sales' }">
          <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5 mr-3" fill="none" viewBox="0 0 24 24" stroke="currentColor">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 19v-6a2 2 0 00-2-2H5a2 2 0 00-2 2v6a2 2 0 002 2h2a2 2 0 002-2zm0 0V9a2 2 0 012-2h2a2 2 0 012 2v10m-6 0a2 2 0 002 2h2a2 2 0 002-2m0 0V5a2 2 0 012-2h2a2 2 0 012 2v14a2 2 0 01-2 2h-2a2 2 0 01-2-2z" />
          </svg>
          <span>Laporan Penjualan</span>
        </NuxtLink>
        
        <NuxtLink to="/reports/finance" class="py-2.5 px-4 rounded-lg mx-2 mb-1 flex items-center transition-colors hover:bg-neutral-100" :class="{ 'bg-primary-50 text-primary-700': $route.path === '/reports/finance' }">
          <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5 mr-3" fill="none" viewBox="0 0 24 24" stroke="currentColor">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 8c-1.657 0-3 .895-3 2s1.343 2 3 2 3 .895 3 2-1.343 2-3 2m0-8c1.11 0 2.08.402 2.599 1M12 8V7m0 1v8m0 0v1m0-1c-1.11 0-2.08-.402-2.599-1M21 12a9 9 0 11-18 0 9 9 0 0118 0z" />
          </svg>
          <span>Laporan Keuangan</span>
        </NuxtLink>
      </nav>
      
      <!-- User Profile Section -->
      <div class="mt-auto border-t border-gray-200">
        <div class="p-4 flex items-center">
          <div class="w-10 h-10 rounded-full bg-primary-500 text-white flex items-center justify-center mr-3">
            <span>RC</span>
          </div>
          <div>
            <div class="font-medium text-gray-900">Rifai Candra</div>
            <div class="text-xs text-gray-500">Admin</div>
          </div>
          <div class="ml-auto">
            <button class="p-1.5 rounded-lg hover:bg-gray-100">
              <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5 text-gray-500" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M17 16l4-4m0 0l-4-4m4 4H7m6 4v1a3 3 0 01-3 3H6a3 3 0 01-3-3V7a3 3 0 013-3h4a3 3 0 013 3v1" />
              </svg>
            </button>
          </div>
        </div>
      </div>
    </div>
    
    <!-- Main Content Area -->
    <div class="flex-1 flex flex-col min-w-0">
      <!-- Desktop Header -->
      <header class="hidden md:flex items-center justify-between bg-white border-b border-gray-200 h-16 px-6 sticky top-0 z-10">
        <div>
          <h1 class="text-xl font-semibold text-gray-800">{{ pageTitle }}</h1>
        </div>
        <div class="flex items-center">
          <!-- Notifications -->
          <button class="p-2 rounded-lg hover:bg-gray-100 relative mr-2">
            <span class="absolute top-1.5 right-1.5 w-2 h-2 bg-red-500 rounded-full"></span>
            <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5 text-gray-600" fill="none" viewBox="0 0 24 24" stroke="currentColor">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M15 17h5l-1.405-1.405A2.032 2.032 0 0118 14.158V11a6.002 6.002 0 00-4-5.659V5a2 2 0 10-4 0v.341C7.67 6.165 6 8.388 6 11v3.159c0 .538-.214 1.055-.595 1.436L4 17h5m6 0v1a3 3 0 11-6 0v-1m6 0H9" />
            </svg>
          </button>
          
          <!-- User menu -->
          <div class="relative">
            <button @click="toggleUserMenu" class="flex items-center space-x-2 focus:outline-none">
              <div class="w-8 h-8 rounded-full bg-primary-500 text-white flex items-center justify-center">
                <span>RC</span>
              </div>
              <span class="hidden lg:block text-sm font-medium text-gray-700">Rifai Candra</span>
              <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5 text-gray-400" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 9l-7 7-7-7" />
              </svg>
            </button>
            
            <!-- Dropdown -->
            <div v-if="userMenuOpen" class="absolute right-0 mt-2 w-48 bg-white rounded-lg shadow-lg py-1 z-20 border border-gray-200">
              <a href="#" class="block px-4 py-2 text-sm text-gray-700 hover:bg-gray-100">Profil</a>
              <a href="#" class="block px-4 py-2 text-sm text-gray-700 hover:bg-gray-100">Pengaturan</a>
              <div class="border-t border-gray-100 my-1"></div>
              <a href="#" class="block px-4 py-2 text-sm text-gray-700 hover:bg-gray-100">Keluar</a>
            </div>
          </div>
        </div>
      </header>
      
      <!-- Page Content -->
      <main class="flex-1 p-5">
        <div class="bg-white rounded-lg shadow-sm p-5">
          <!-- Page content -->
          <slot />
        </div>
      </main>
      
      <!-- Footer -->
      <footer class="bg-white border-t border-gray-200 py-4 px-6 text-center text-sm text-gray-500">
        &copy; 2025 Koperasi App. Hak Cipta Dilindungi.
      </footer>
    </div>
    
    <!-- Backdrop Overlay (Mobile) -->
    <div v-if="sidebarOpen" @click="toggleSidebar" class="md:hidden fixed inset-0 bg-black bg-opacity-30 z-10"></div>
  </div>
</template>

<script setup>
const route = useRoute();
const sidebarOpen = ref(false);
const userMenuOpen = ref(false);

const toggleSidebar = () => {
  sidebarOpen.value = !sidebarOpen.value;
};

const toggleUserMenu = () => {
  userMenuOpen.value = !userMenuOpen.value;
};

// Close user menu when clicking outside
onMounted(() => {
  document.addEventListener('click', (e) => {
    if (userMenuOpen.value) {
      userMenuOpen.value = false;
    }
  });
});

// Compute page title based on route
const pageTitle = computed(() => {
  const path = route.path;
  if (path === '/dashboard') return 'Dashboard';
  if (path === '/products') return 'Manajemen Produk';
  if (path === '/transactions') return 'Transaksi';
  if (path === '/members') return 'Anggota Koperasi';
  if (path === '/reports/sales') return 'Laporan Penjualan';
  if (path === '/reports/finance') return 'Laporan Keuangan';
  return 'Koperasi App';
});
</script>