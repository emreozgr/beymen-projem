<template>
  <div class="container-fluid mt-4">
    <div class="row">
      
      <div class="col-md-3 mb-4">
        <div class="card shadow-sm sticky-top" style="top: 20px; max-height: 90vh; overflow-y: auto;">
          <div class="card-header bg-dark text-white">
            <h5 class="mb-0"><i class="bi bi-grid-fill me-2"></i>Proje Mimarisi</h5>
          </div>
          
          <div class="list-group list-group-flush">
            <button 
              v-for="(item, index) in componentList" 
              :key="index"
              class="list-group-item list-group-item-action d-flex justify-content-between align-items-center"
              :class="{ active: selectedComponentName === item.name }"
              @click="selectComponent(item)"
            >
              <div>
                <i v-if="item.type === 'layout'" class="bi bi-layout-text-window-reverse me-2 text-muted"></i>
                <i v-if="item.type === 'component'" class="bi bi-box-seam me-2 text-primary"></i>
                <i v-if="item.type === 'page'" class="bi bi-file-earmark-text me-2 text-success"></i>
                <strong>{{ item.name }}</strong>
                <div class="small text-muted" style="font-size: 0.75rem;">{{ item.desc }}</div>
              </div>
              
              <span v-if="Object.keys(item.props).length > 0" class="badge bg-warning text-dark rounded-pill" style="font-size: 0.65rem;">Prop</span>
            </button>
          </div>
        </div>
      </div>

      <div class="col-md-9">
        <div class="card shadow-sm">
          <div class="card-header d-flex justify-content-between align-items-center bg-white">
            <h5 class="mb-0">
              Önizleme: <span class="text-primary fw-bold">{{ selectedComponentName || 'Seçim Yapılmadı' }}</span>
            </h5>
            <span class="badge bg-light text-dark border">Canlı Render Modu</span>
          </div>
          
          <div class="card-body bg-light d-flex flex-column align-items-center justify-content-start p-0" style="min-height: 600px; overflow-x: hidden;">
            
            <div class="w-100 bg-white border-bottom p-2 text-center text-muted small mb-3">
              ⬇️ Bileşen Aşağıda Render Ediliyor ⬇️
            </div>

            <div class="component-wrapper w-100 px-4 pb-5">
              <component 
                v-if="selectedComponent"
                :is="selectedComponent" 
                v-bind="selectedProps"
              ></component>

              <div v-else class="text-center text-muted mt-5 pt-5">
                <i class="bi bi-cursor-fill display-4"></i>
                <h3 class="mt-3">Listeden bir bileşen seçin</h3>
                <p>Hocanın istediği "Prop ile dışarıdan veri alma" ve "Dinamik Gösterim" yapısı burada çalışmaktadır.</p>
              </div>
            </div>

          </div>

          <div class="card-footer bg-dark text-white" v-if="selectedComponent">
            <h6 class="text-warning mb-2" style="font-size: 0.9rem;">
              <i class="bi bi-code-slash me-2"></i>
              Parent'tan (Bu sayfadan) gönderilen Props Verisi:
            </h6>
            <pre class="m-0 text-light bg-secondary p-2 rounded" style="font-size: 0.8rem;">{{ JSON.stringify(selectedProps, null, 2) }}</pre>
          </div>
        </div>
      </div>

    </div>
  </div>
</template>

<script setup>
import { ref, shallowRef } from 'vue';

// --- 1. DOSYA YAPISINDAKİ TÜM BİLEŞENLERİ İÇE AKTAR ---
// Components
import TheHeader from '@/components/TheHeader.vue';
import TheFooter from '@/components/TheFooter.vue';
import ProductCard from '@/components/ProductCard.vue';
// Views (Sayfalar)
import Home from '@/views/Home.vue';
import Login from '@/views/Login.vue';
import Register from '@/views/Register.vue';
import Cart from '@/views/Cart.vue';
import Favorites from '@/views/Favorites.vue';
// import Category from '@/views/Category.vue'; // Kategori sayfası dinamik olduğu için burada hata verebilir, gerekirse ekleriz

// --- 2. STATE ---
const selectedComponent = shallowRef(null); 
const selectedComponentName = ref('');
const selectedProps = ref({});

// --- 3. LİSTE (TÜM DOSYALAR BURADA) ---
const componentList = [
  // --- TEMEL PARÇALAR (LAYOUT) ---
  {
    name: 'TheHeader',
    desc: 'Sabit Üst Menü',
    type: 'layout',
    component: TheHeader,
    props: {}
  },
  {
    name: 'TheFooter',
    desc: 'Sabit Alt Menü',
    type: 'layout',
    component: TheFooter,
    props: {}
  },
  
  // --- BİLEŞENLER (COMPONENTS) ---
  {
    name: 'ProductCard (Örnek 1)',
    desc: 'Ürün Kartı - Gömlek',
    type: 'component',
    component: ProductCard,
    // HOCANIN İSTEDİĞİ PROP KULLANIMI:
    props: {
      product: {
        id: 101,
        imageUrl: 'https://picsum.photos/400/533?image=100',
        brand: 'MAVİ',
        name: 'Mavi İpek Gömlek (Showcase)',
        price: '4.500 TL'
      }
    }
  },
  {
    name: 'ProductCard (Örnek 2)',
    desc: 'Ürün Kartı - Çanta',
    type: 'component',
    component: ProductCard,
    props: {
      product: {
        id: 102,
        imageUrl: 'https://picsum.photos/400/533?image=200',
        brand: 'GUCCI',
        name: 'Deri Çanta (Showcase)',
        price: '12.000 TL'
      }
    }
  },

  // --- SAYFALAR (VIEWS) ---
  {
    name: 'Home View',
    desc: 'Anasayfa (Tüm site)',
    type: 'page',
    component: Home,
    props: {}
  },
  {
    name: 'Login View',
    desc: 'Giriş Yap Sayfası',
    type: 'page',
    component: Login,
    props: {}
  },
  {
    name: 'Register View',
    desc: 'Üye Ol Sayfası',
    type: 'page',
    component: Register,
    props: {}
  },
  {
    name: 'Cart View',
    desc: 'Sepetim Sayfası',
    type: 'page',
    component: Cart,
    props: {}
  },
  {
    name: 'Favorites View',
    desc: 'Favorilerim Sayfası',
    type: 'page',
    component: Favorites,
    props: {}
  }
];

// --- 4. SEÇİM FONKSİYONU ---
function selectComponent(item) {
  selectedComponent.value = item.component;
  selectedComponentName.value = item.name;
  selectedProps.value = item.props;
}
</script>

<style scoped>
/* Liste ikonları için renkler */
.text-primary { color: #0d6efd !important; }
.text-success { color: #198754 !important; }
.text-muted { color: #6c757d !important; }

/* Aktif eleman stili */
.list-group-item.active {
  background-color: #212529;
  border-color: #212529;
  color: #fff;
}
.list-group-item.active .text-muted,
.list-group-item.active .text-primary,
.list-group-item.active .text-success {
  color: #ddd !important; /* Aktifken ikonları açık renk yap */
}
</style>