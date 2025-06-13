<script setup lang="ts">
const { $supabase } = useNuxtApp()

const anggota = ref<any>([])
const loading = ref(true)
const errorMsg = ref('')

onMounted(async () => {
  try {
    const { data, error } = await $supabase
      .from('members')
      .select('*')

    if (error) {
      console.error('Supabase error:', error.message)
      errorMsg.value = 'Gagal mengambil data anggota. Silakan coba lagi nanti.'
      return
    }

    anggota.value = data
  } catch (err) {
    console.error('Unexpected error:', err)
    errorMsg.value = 'Terjadi kesalahan tak terduga. 😵‍💫'
  } finally {
    loading.value = false
  }
})
</script>

<template>
  <div v-if="loading">🚀 Loading data anggota...</div>

  <div v-else-if="errorMsg">
    ⚠️ {{ errorMsg }}
  </div>

  <ul v-else>
    <li v-for="item in anggota" :key="item.id">
      🛍️ {{ item.name_produk }}
    </li>
  </ul>
</template>
