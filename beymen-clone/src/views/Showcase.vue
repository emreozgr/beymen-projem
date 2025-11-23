<template>
  <div class="container-fluid mt-4">
    <div class="row">
      
      <div class="col-md-3 border-end">
        <h4 class="mb-3">Bileşen Listesi</h4>
        <div class="list-group">
          <button 
            v-for="(item, index) in componentList" 
            :key="index"
            class="list-group-item list-group-item-action"
            :class="{ active: selectedComponent === item.component }"
            @click="selectComponent(item)"
          >
            {{ item.name }}
          </button>
        </div>
      </div>

      <div class="col-md-9">
        <h4 class="mb-3">Önizleme: {{ selectedComponentName }}</h4>
        <div class="p-4 border bg-light rounded">
          
          <component 
            :is="selectedComponent" 
            v-bind="selectedProps"
          ></component>

        </div>
        
        <div class="mt-4" v-if="selectedProps">
          <h6>Gönderilen Prop Verileri:</h6>
          <pre class="bg-dark text-white p-3 rounded">{{ selectedProps }}</pre>
        </div>
      </div>

    </div>
  </div>
</template>

<script setup>
import { ref, shallowRef } from 'vue';

// --- 1. Bileşenleri İçe Aktar (Import) ---
// Göstermek istediğin tüm bileşenleri buraya import etmelisin
import ProductCard from '@/components/ProductCard.vue';
import TheHeader from '@/components/TheHeader.vue';
import TheFooter from '@/components/TheFooter.vue';
// Eğer Login formu bileşen halindeyse onu da ekleyebilirsin
// import LoginForm from '@/components/LoginForm.vue'; 

// --- 2. Seçili Olanı Takip Etme ---
const selectedComponent = shallowRef(null); // O an ekranda görünen bileşen
const selectedComponentName = ref('Lütfen soldan bir seçim yapın');
const selectedProps = ref({});

// --- 3. Bileşen Listesi ve Prop Verileri ---
// Hocanın "Veriyi dışarıdan prop ile alsın" dediği kısım burası.
// Her bileşen için sahte verileri burada tanımlıyoruz.
const componentList = [
  {
    name: 'Header (Üst Menü)',
    component: TheHeader,
    props: {} // Header genelde prop almaz ama gerekirse buraya yazılır
  },
  {
    name: 'Footer (Alt Menü)',
    component: TheFooter,
    props: {}
  },
  {
    name: 'Ürün Kartı (Örnek 1)',
    component: ProductCard,
    // İşte PROP kullanımı: Ürün verisini buradan gönderiyoruz
    props: {
      product: {
        id: 101,
        title: 'Mavi İpek Gömlek',
        priceText: '4.500 TL',
        imageUrl: 'https://picsum.photos/400/533?image=100'
      }
    }
  },
  {
    name: 'Ürün Kartı (Örnek 2)',
    component: ProductCard,
    // Aynı bileşeni farklı veriyle test ediyoruz
    props: {
      product: {
        id: 102,
        title: 'Deri Çanta',
        priceText: '12.000 TL',
        imageUrl: 'https://picsum.photos/400/533?image=200'
      }
    }
  }
];

// --- 4. Seçim Fonksiyonu ---
function selectComponent(item) {
  selectedComponent.value = item.component;
  selectedComponentName.value = item.name;
  selectedProps.value = item.props;
}
</script>