<template>
  <div class="w-full max-w-4xl mx-auto flex flex-col md:flex-row rounded-2xl overflow-hidden shadow-2xl">
    <!-- Left Side - Illustration (only visible on md screens and above) -->
    <div class="hidden md:flex md:w-5/12 bg-gradient-to-br from-primary-500 to-primary-700 p-8 justify-center items-center">
      <div class="text-center text-white">
        <div class="text-7xl mb-6">
          <svg xmlns="http://www.w3.org/2000/svg" class="w-20 h-20 mx-auto" fill="none" viewBox="0 0 24 24" stroke="currentColor">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="1.5" d="M13.5 21v-7.5a.75.75 0 01.75-.75h3a.75.75 0 01.75.75V21m-4.5 0H2.36m11.14 0H18m0 0h3.64m-1.39 0V9.349m-16.5 11.65V9.35m0 0a3.001 3.001 0 003.75-.615A2.993 2.993 0 009.75 9.75c.896 0 1.7-.393 2.25-1.016a2.993 2.993 0 002.25 1.016c.896 0 1.7-.393 2.25-1.016a3.001 3.001 0 003.75.614m-16.5 0a3.004 3.004 0 01-.621-4.72L4.318 3.44A1.5 1.5 0 015.378 3h13.243a1.5 1.5 0 011.06.44l1.19 1.189a3 3 0 01-.621 4.72m-13.5 8.65h3.75a.75.75 0 00.75-.75V13.5a.75.75 0 00-.75-.75H6.75a.75.75 0 00-.75.75v3.75c0 .415.336.75.75.75z" />
          </svg>
        </div>
        <h2 class="text-3xl font-bold mb-3">Koperasi App</h2>
        <p class="text-white/80">Solusi digital untuk pengelolaan koperasi Anda</p>
      </div>
    </div>
    
    <!-- Right Side - Login Form -->
    <div class="w-full md:w-7/12 p-8 bg-white">
      <!-- Mobile Logo (only visible on small screens) -->
      <div class="md:hidden text-center mb-8">
        <svg xmlns="http://www.w3.org/2000/svg" class="w-16 h-16 mx-auto text-primary-500 mb-4" fill="none" viewBox="0 0 24 24" stroke="currentColor">
          <path stroke-linecap="round" stroke-linejoin="round" stroke-width="1.5" d="M13.5 21v-7.5a.75.75 0 01.75-.75h3a.75.75 0 01.75.75V21m-4.5 0H2.36m11.14 0H18m0 0h3.64m-1.39 0V9.349m-16.5 11.65V9.35m0 0a3.001 3.001 0 003.75-.615A2.993 2.993 0 009.75 9.75c.896 0 1.7-.393 2.25-1.016a2.993 2.993 0 002.25 1.016c.896 0 1.7-.393 2.25-1.016a3.001 3.001 0 003.75.614m-16.5 0a3.004 3.004 0 01-.621-4.72L4.318 3.44A1.5 1.5 0 015.378 3h13.243a1.5 1.5 0 011.06.44l1.19 1.189a3 3 0 01-.621 4.72m-13.5 8.65h3.75a.75.75 0 00.75-.75V13.5a.75.75 0 00-.75-.75H6.75a.75.75 0 00-.75.75v3.75c0 .415.336.75.75.75z" />
        </svg>
        <h1 class="text-2xl font-bold text-gray-800">Koperasi App</h1>
      </div>
      
      <!-- Login Header -->
      <div class="mb-8">
        <h2 class="text-2xl font-bold text-gray-800">Selamat Datang</h2>
        <p class="text-gray-500 mt-2">Masuk ke akun Anda untuk melanjutkan</p>
      </div>

      <!-- Login Form -->
      <form @submit.prevent="onSubmit">
        <div class="mb-4">
          <label for="email" class="block text-sm font-medium text-gray-700 mb-1">Email</label>
          <div class="relative">
            <div class="absolute inset-y-0 left-0 pl-3 flex items-center pointer-events-none">
              <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5 text-gray-400" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M16 12a4 4 0 10-8 0 4 4 0 008 0zm0 0v1.5a2.5 2.5 0 005 0V12a9 9 0 10-9 9m4.5-1.206a8.959 8.959 0 01-4.5 1.207" />
              </svg>
            </div>
            <input 
              id="email" 
              v-model="formState.email" 
              type="email" 
              class="pl-10 w-full rounded-lg border border-gray-300 px-4 py-2 focus:outline-none focus:ring-2 focus:ring-primary-500 focus:border-transparent"
              placeholder="email@anda.com" 
              autocomplete="email"
              required
            />
          </div>
        </div>

        <div class="mb-4">
          <label for="password" class="block text-sm font-medium text-gray-700 mb-1">Password</label>
          <div class="relative">
            <div class="absolute inset-y-0 left-0 pl-3 flex items-center pointer-events-none">
              <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5 text-gray-400" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 15v2m-6 4h12a2 2 0 002-2v-6a2 2 0 00-2-2H6a2 2 0 00-2 2v6a2 2 0 002 2zm10-10V7a4 4 0 00-8 0v4h8z" />
              </svg>
            </div>
            <input 
              id="password" 
              v-model="formState.password" 
              :type="showPassword ? 'text' : 'password'" 
              class="pl-10 w-full rounded-lg border border-gray-300 px-4 py-2 focus:outline-none focus:ring-2 focus:ring-primary-500 focus:border-transparent"
              placeholder="Masukkan password" 
              autocomplete="current-password"
              required
            />
            <button 
              type="button"
              class="absolute inset-y-0 right-0 pr-3 flex items-center"
              @click="showPassword = !showPassword"
            >
              <svg v-if="showPassword" xmlns="http://www.w3.org/2000/svg" class="h-5 w-5 text-gray-400" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M15 12a3 3 0 11-6 0 3 3 0 016 0z" />
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M2.458 12C3.732 7.943 7.523 5 12 5c4.478 0 8.268 2.943 9.542 7-1.274 4.057-5.064 7-9.542 7-4.477 0-8.268-2.943-9.542-7z" />
              </svg>
              <svg v-else xmlns="http://www.w3.org/2000/svg" class="h-5 w-5 text-gray-400" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M13.875 18.825A10.05 10.05 0 0112 19c-4.478 0-8.268-2.943-9.543-7a9.97 9.97 0 011.563-3.029m5.858.908a3 3 0 114.243 4.243M9.878 9.878l4.242 4.242M9.88 9.88l-3.29-3.29m7.532 7.532l3.29 3.29M3 3l3.59 3.59m0 0A9.953 9.953 0 0112 5c4.478 0 8.268 2.943 9.543 7a10.025 10.025 0 01-4.132 5.411m0 0L21 21" />
              </svg>
            </button>
          </div>
        </div>

        <div class="flex justify-between items-center mt-6">
          <div class="flex items-center">
            <input 
              id="remember" 
              v-model="formState.remember" 
              type="checkbox" 
              class="h-4 w-4 text-primary-600 focus:ring-primary-500 border-gray-300 rounded"
            />
            <label for="remember" class="ml-2 block text-sm text-gray-700">Ingat saya</label>
          </div>
          <a href="/forgot-password" class="text-sm font-medium text-primary-600 hover:text-primary-700">
            Lupa password?
          </a>
        </div>

        <button 
          type="submit" 
          class="w-full bg-gradient-to-r from-primary-600 to-primary-500 hover:from-primary-700 hover:to-primary-600 text-white font-medium py-2.5 px-4 rounded-lg mt-6 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-primary-500 transition-all duration-300"
          :disabled="isLoading"
        >
          <span v-if="isLoading" class="flex items-center justify-center">
            <svg class="animate-spin -ml-1 mr-2 h-4 w-4 text-white" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24">
              <circle class="opacity-25" cx="12" cy="12" r="10" stroke="currentColor" stroke-width="4"></circle>
              <path class="opacity-75" fill="currentColor" d="M4 12a8 8 0 018-8V0C5.373 0 0 5.373 0 12h4zm2 5.291A7.962 7.962 0 014 12H0c0 3.042 1.135 5.824 3 7.938l3-2.647z"></path>
            </svg>
            Memproses...
          </span>
          <span v-else>Masuk</span>
        </button>
      </form>

      <!-- Register link -->
      <div class="text-center mt-8 pt-4 border-t border-gray-100">
        <p class="text-gray-600">
          Belum punya akun?
          <a href="/register" class="ml-1 text-primary-600 font-medium hover:text-primary-700">
            Daftar sekarang
          </a>
        </p>
      </div>
    </div>
  </div>
</template>

<script setup>
definePageMeta({
  layout: 'login'
})

const formState = ref({
  email: '',
  password: '',
  remember: false
});

const isLoading = ref(false);
const showPassword = ref(false);

const onSubmit = async () => {
  try {
    isLoading.value = true;
    
    // Simulasi login delay
    await new Promise(resolve => setTimeout(resolve, 1000));
    
    // Di sini Anda dapat menambahkan logika autentikasi
    // contoh: await useAuth().login(formState.value)
    
    // Redirect setelah login
    await navigateTo('/dashboard');
  } catch (error) {
    // Handle error
    console.error('Login error:', error);
  } finally {
    isLoading.value = false;
  }
};
</script>

<style>
/* Pastikan warna primary tersedia untuk referensi */
.text-primary-500 {
  color: #4CAF50;
}
.text-primary-600 {
  color: #43A047;
}
.text-primary-700 {
  color: #388E3C;
}
.bg-primary-500 {
  background-color: #4CAF50;
}
.bg-primary-600 {
  background-color: #43A047;
}
.bg-primary-700 {
  background-color: #388E3C;
}
.from-primary-500 {
  --tw-gradient-from: #4CAF50;
}
.from-primary-600 {
  --tw-gradient-from: #43A047;
}
.from-primary-700 {
  --tw-gradient-from: #388E3C;
}
.to-primary-500 {
  --tw-gradient-to: #4CAF50;
}
.to-primary-600 {
  --tw-gradient-to: #43A047;
}
.to-primary-700 {
  --tw-gradient-to: #388E3C;
}
.focus\:ring-primary-500:focus {
  --tw-ring-color: #4CAF50;
}
</style>