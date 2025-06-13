andra/development/toko-koperasi/pages/products/index.vue
<template>
  <div>
    <div class="flex justify-between items-center mb-6">
      <h1 class="text-2xl font-bold text-gray-800">Manajemen Produk</h1>
      <div class="flex space-x-3">
        <button 
          v-if="produkTerpilih.length > 0" 
          @click="showModalBulkHapus = true"
          class="px-4 py-2 bg-red-50 text-red-600 rounded-lg border border-red-200 hover:bg-red-100 transition-colors flex items-center"
        >
          <svg xmlns="http://www.w3.org/2000/svg" class="h-4 w-4 mr-1" fill="none" viewBox="0 0 24 24" stroke="currentColor">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 7l-.867 12.142A2 2 0 0116.138 21H7.862a2 2 0 01-1.995-1.858L5 7m5 4v6m4-6v6m1-10V4a1 1 0 00-1-1h-4a1 1 0 00-1 1v3M4 7h16" />
          </svg>
          Hapus {{ produkTerpilih.length }} item
        </button>
        <button 
          @click="bukaFormProduk()"
          class="px-4 py-2 bg-primary-600 text-white rounded-lg hover:bg-primary-700 transition-colors flex items-center"
        >
          <svg xmlns="http://www.w3.org/2000/svg" class="h-4 w-4 mr-1" fill="none" viewBox="0 0 24 24" stroke="currentColor">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 6v6m0 0v6m0-6h6m-6 0H6" />
          </svg>
          Tambah Produk
        </button>
      </div>
    </div>

    <!-- Filter dan Pencarian -->
    <FilterPencarian 
      v-model:kataPencarian="kataPencarian"
      v-model:urutkan="urutkan"
    />

    <!-- Tabel Produk -->
    <TabelProduk 
      :produk="produkHalaman"
      :loading="loading"
      :semuaTerpilih="semuaTerpilih"
      :produkTerpilih="produkTerpilih"
      @toggle-semua="togglePilihSemua"
      @update:terpilih="produkTerpilih = $event"
      @edit="bukaFormProduk($event)"
      @hapus="konfirmasiHapus($event)"
      @tambah="bukaFormProduk()"
    />

    <!-- Paginasi -->
    <PaginasiProduk
      v-if="!loading && produkFilter.length > 0"
      :itemAwal="itemAwal"
      :itemAkhir="itemAkhir"
      :totalItem="produkFilter.length"
      v-model:halamanAktif="halamanAktif"
      :totalHalaman="totalHalaman"
    />

    <!-- Form Produk -->
    <FormProduk
      :show="showFormProduk"
      :modeEdit="modeEdit"
      :produk="produkDiedit"
      @tutup="showFormProduk = false"
      @simpan="simpanProduk"
    />

    <!-- Modal Konfirmasi Hapus -->
    <ModalKonfirmasiHapus
      :show="showModalHapus"
      :loading="prosesHapus"
      @tutup="showModalHapus = false"
      @konfirmasi="hapusProduk"
    />

    <!-- Modal Konfirmasi Hapus Bulk -->
    <ModalKonfirmasiBulkHapus
      :show="showModalBulkHapus"
      :loading="prosesHapus"
      :jumlah="produkTerpilih.length"
      @tutup="showModalBulkHapus = false"
      @konfirmasi="hapusBanyakProduk"
    />

    <!-- Notifikasi Toast -->
    <Notifikasi
      :items="notifikasi"
      @hapus="hapusNotifikasi"
    />
  </div>
</template>

<script setup>
import { ref, computed, onMounted, watch } from 'vue';
import FormProduk from '~/components/products/FormProduk.vue';
import TabelProduk from '~/components/products/TabelProduk.vue';
import ModalKonfirmasiHapus from '~/components/products/ModalKonfirmasiHapus.vue';
import ModalKonfirmasiBulkHapus from '~/components/products/ModalKonfirmasiBulkHapus.vue';
import FilterPencarian from '~/components/products/FilterPencarian.vue';
import PaginasiProduk from '~/components/products/PaginasiProduk.vue';
import Notifikasi from '~/components/ui/Notifikasi.vue';

const { $supabase } = useNuxtApp();

// State
const produk = ref([]);
const loading = ref(true);
const prosesHapus = ref(false);
const kataPencarian = ref('');
const urutkan = ref('name_produk-asc');
const produkTerpilih = ref([]);
const produkDihapus = ref(null);
const halamanAktif = ref(1);
const itemPerHalaman = 5;
const showFormProduk = ref(false);
const showModalHapus = ref(false);
const showModalBulkHapus = ref(false);
const modeEdit = ref(false);
const notifikasi = ref([]);
const produkDiedit = ref(null);

// Filter produk
const produkFilter = computed(() => {
  let hasil = [...produk.value];
  
  // Terapkan filter pencarian
  if (kataPencarian.value) {
    const kata = kataPencarian.value.toLowerCase();
    hasil = hasil.filter(item => 
      item.name_produk?.toLowerCase().includes(kata) || 
      item.kode_produk?.toLowerCase().includes(kata)
    );
  }
  
  // Terapkan pengurutan
  const [field, arah] = urutkan.value.split('-');
  
  hasil.sort((a, b) => {
    let perbandingan = 0;
    
    if (field === 'name_produk') {
      perbandingan = a.name_produk.localeCompare(b.name_produk);
    } else if (field === 'harga_jual') {
      perbandingan = parseFloat(a.harga_jual) - parseFloat(b.harga_jual);
    } else if (field === 'stok') {
      perbandingan = a.stok - b.stok;
    }
    
    return arah === 'asc' ? perbandingan : -perbandingan;
  });
  
  return hasil;
});

// Paginasi
const totalHalaman = computed(() => {
  return Math.ceil(produkFilter.value.length / itemPerHalaman);
});

const produkHalaman = computed(() => {
  const mulai = (halamanAktif.value - 1) * itemPerHalaman;
  const akhir = mulai + itemPerHalaman;
  return produkFilter.value.slice(mulai, akhir);
});

const itemAwal = computed(() => {
  return produkFilter.value.length === 0
    ? 0
    : (halamanAktif.value - 1) * itemPerHalaman + 1;
});

const itemAkhir = computed(() => {
  return Math.min(halamanAktif.value * itemPerHalaman, produkFilter.value.length);
});

// Toggle pilih semua
const semuaTerpilih = computed({
  get: () => {
    return produkHalaman.value.length > 0 && 
      produkTerpilih.value.length === produkHalaman.value.length;
  },
  set: (value) => {
    if (value) {
      produkTerpilih.value = produkHalaman.value.map(item => item.id_produk);
    } else {
      produkTerpilih.value = [];
    }
  }
});

// Methods
const ambilDataProduk = async () => {
  try {
    loading.value = true;
    const { data, error } = await $supabase.from('products').select('*');
    
    if (error) throw error;
    
    produk.value = data || [];
  } catch (error) {
    tampilkanNotifikasi('Gagal memuat produk: ' + error.message, 'error');
  } finally {
    loading.value = false;
  }
};

const bukaFormProduk = (item = null) => {
  if (item) {
    // Mode edit
    modeEdit.value = true;
    produkDiedit.value = item;
  } else {
    // Mode tambah baru
    modeEdit.value = false;
    produkDiedit.value = {
      id_produk: null,
      kode_produk: '',
      name_produk: '',
      harga_beli: 0,
      harga_jual: 0,
      stok: 0,
      tanggal_masuk: new Date().toISOString().split('T')[0]
    };
  }
  
  showFormProduk.value = true;
};

const simpanProduk = async (formData) => {
  try {
    if (modeEdit.value) {
      // Update produk yang ada
      const { id_produk, ...dataProduk } = formData;
      const { error } = await $supabase
        .from('products')
        .update(dataProduk)
        .eq('id_produk', id_produk);
      
      if (error) throw error;
      
      tampilkanNotifikasi('Produk berhasil diperbarui!', 'sukses');
    } else {
      // Tambah produk baru
      const { id_produk, ...dataProduk } = formData;
      const { error } = await $supabase
        .from('products')
        .insert([dataProduk]);
      
      if (error) throw error;
      
      tampilkanNotifikasi('Produk baru berhasil ditambahkan!', 'sukses');
    }
    
    // Refresh daftar produk dan tutup form
    showFormProduk.value = false;
    ambilDataProduk();
  } catch (error) {
    tampilkanNotifikasi(
      `Gagal ${modeEdit.value ? 'memperbarui' : 'menambahkan'} produk: ${error.message}`, 
      'error'
    );
  }
};

const konfirmasiHapus = (item) => {
  produkDihapus.value = item;
  showModalHapus.value = true;
};

const hapusProduk = async () => {
  if (!produkDihapus.value) return;
  
  try {
    prosesHapus.value = true;
    
    const { error } = await $supabase
      .from('products')
      .delete()
      .eq('id_produk', produkDihapus.value.id_produk);
    
    if (error) throw error;
    
    // Hapus dari daftar produk terpilih jika sudah dipilih
    produkTerpilih.value = produkTerpilih.value.filter(id => id !== produkDihapus.value.id_produk);
    
    tampilkanNotifikasi('Produk berhasil dihapus!', 'sukses');
  } catch (error) {
    tampilkanNotifikasi('Gagal menghapus produk: ' + error.message, 'error');
  } finally {
    prosesHapus.value = false;
    showModalHapus.value = false;
    produkDihapus.value = null;
    ambilDataProduk();
  }
};

const hapusBanyakProduk = async () => {
  if (produkTerpilih.value.length === 0) return;
  
  try {
    prosesHapus.value = true;
    
    const { error } = await $supabase
      .from('products')
      .delete()
      .in('id_produk', produkTerpilih.value);
    
    if (error) throw error;
    
    tampilkanNotifikasi(`${produkTerpilih.value.length} produk berhasil dihapus!`, 'sukses');
    produkTerpilih.value = [];
  } catch (error) {
    tampilkanNotifikasi('Gagal menghapus produk: ' + error.message, 'error');
  } finally {
    prosesHapus.value = false;
    showModalBulkHapus.value = false;
    ambilDataProduk();
  }
};

const togglePilihSemua = () => {
  semuaTerpilih.value = !semuaTerpilih.value;
};

const tampilkanNotifikasi = (pesan, tipe = 'sukses') => {
  const item = { pesan, tipe };
  notifikasi.value.push(item);
  
  // Otomatis hapus setelah 5 detik
  setTimeout(() => {
    const index = notifikasi.value.indexOf(item);
    if (index !== -1) {
      notifikasi.value.splice(index, 1);
    }
  }, 5000);
};

const hapusNotifikasi = (index) => {
  notifikasi.value.splice(index, 1);
};

// Reset halaman saat filter berubah
watch([kataPencarian, urutkan], () => {
  halamanAktif.value = 1;
});

// Ambil data produk saat komponen dimuat
onMounted(() => {
  ambilDataProduk();
});
</script>