<template>
  <v-app-bar flat density="compact" :height="barHeight" class="top-info-bar" elevation="4">
    <v-container class="d-flex justify-space-between align-center pl-0">
      <v-app-bar-title>
        <div class="d-flex align-center">
          <v-img :src="logo" :height="logoHeight" :max-width="logoWidth" alt="Logo" class="d-inline-block" />
          <span class="logo-text">Здружение на приватни лекари на Р.Македонија<br />Прв Симпозиум „Како до подобро
            здравје“</span>
        </div>
      </v-app-bar-title>

      <div class="text-right pr-5 mr-4 info-section" style="border-right: 1px solid black">
        <p>
          17-19 Април 2026 год.<br />
          Хотел Дрим Струга
        </p>
      </div>

      <a href="https://maps.app.goo.gl/5qJc3Phb38ACxsoU9" target="_blank"
        class="text-caption text-white text-decoration-none location-link">
        📍 Локација
      </a>
    </v-container>
  </v-app-bar>
  <v-app-bar :elevation="4" class="app-bar" height="60" fixed density="compact">
    <v-container class="d-flex align-center">
      <!-- Desktop Navigation -->
      <v-tabs v-model="activeTab" class="d-none d-md-flex" align-tabs="center" color="accent" slider-color="accent"
        @update:model-value="handleTabChange">
        <v-tab v-for="(tab, index) in tabs" :key="index" :value="index" class="nav-tab pa-3" :prepend-icon="tab.icon">
          {{ tab.label }}
        </v-tab>
      </v-tabs>
      <v-spacer></v-spacer>
      <!-- Mobile Menu -->
      <v-app-bar-nav-icon class="d-md-none" @click="drawer = !drawer" color="white"></v-app-bar-nav-icon>
    </v-container>
  </v-app-bar>

  <!-- Mobile Navigation Drawer -->
  <v-navigation-drawer v-model="drawer" temporary location="right" class="mobile-nav">
    <v-list>
      <v-list-item v-for="(tab, index) in tabs" :key="index" @click="navigateToTab(tab.route)"
        :class="{ 'active-mobile-item': $route.name === tab.route }">
        <template v-slot:prepend>
          <v-icon>{{ tab.icon }}</v-icon>
        </template>
        <v-list-item-title>{{ tab.label }}</v-list-item-title>
      </v-list-item>
    </v-list>
    <a href="https://maps.app.goo.gl/5qJc3Phb38ACxsoU9" target="_blank"
      class="text-caption text-white text-decoration-none pl-5">
      📍 Локација
    </a>
  </v-navigation-drawer>
</template>

<script setup>
import { computed, ref, watch } from 'vue';
import { useRoute, useRouter } from "vue-router";
import logo from '@/assets/logozplrm.png'
import { useDisplay } from "vuetify/lib/composables/index.js";

const router = useRouter()
const route = useRoute()
const { xs, sm, mdAndUp } = useDisplay();

const drawer = ref(false)
const activeTab = ref(0)

const tabs = [
  { label: 'За настанот', icon: 'mdi-information-outline', route: 'AboutPage' },
  { label: 'За нас', icon: 'mdi-chair-school', route: 'SymposiumPage' },
  { label: 'Апстракти', icon: 'mdi-file-document', route: 'AbstractsPage' },
  { label: 'Програма', icon: 'mdi-calendar-text', route: 'ProgramPage' },
  { label: 'Работилници', icon: 'mdi-presentation', route: 'WorkshopPage' },
  { label: 'Регистрација', icon: 'mdi-cash', route: 'RegistrationPage' },
  { label: 'Сместување', icon: 'mdi-bed', route: 'HotelPage' },
  { label: 'Зачленување', icon: 'mdi-account-plus', route: 'MembershipPage' },
]

// Watch route changes to update active tab
watch(route, (newRoute) => {
  const tabIndex = tabs.findIndex(tab => tab.route === newRoute.name)
  if (tabIndex !== -1) {
    activeTab.value = tabIndex
  }
}, { immediate: true })



const handleTabChange = (tabIndex) => {
  navigateToTab(tabs[tabIndex].route)
}

const navigateToTab = (routeName) => {
  router.push({ name: routeName })
  drawer.value = false
}

const barHeight = computed(() => {
  return xs.value || sm.value ? 70 : 90;
});

const logoHeight = computed(() => {
  return xs.value || sm.value ? 40 : 60;
});

const logoWidth = computed(() => {
  return xs.value || sm.value ? 60 : 100;
});
</script>

<style scoped>
.app-bar {
  background: linear-gradient(135deg, #207cbd 0%, #005d98 100%) !important;
}

.logo {
  color: #FFFFFF !important;
}

.v-slide-group {
  color: white !important;
}

.nav-tab {
  color: white !important;
  font-weight: 500 !important;
  text-transform: uppercase !important;
  border-radius: 20px !important;
  margin: 0 10px !important;
  transition: all 0.3s ease !important;
}

.nav-tab:hover {
  background: rgba(255, 255, 255, 0.1) !important;
}

.main-content {
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  min-height: 100vh;
}

.mobile-nav {
  background: linear-gradient(135deg, #4a5568 0%, #2d3748 100%) !important;
}

.mobile-nav :deep(.v-list-item) {
  color: white !important;
}

.active-mobile-item {
  background: rgba(129, 230, 217, 0.2) !important;
  border-left: 4px solid #81e6d9 !important;
}

.top-info-bar {
  background: #004679 !important;
  color: white !important;
  font-size: 0.875rem;
  z-index: 10;
}

.info-section {
  font-size: 1.1rem;
  line-height: 1.2rem;
}

.logo-text {
  font-size: 1.1rem;
  line-height: 1.2rem;
}

.lang-toggle {
  max-width: 100%;
  overflow: hidden;
  flex-shrink: 0;
}

.lang-toggle .v-btn {
  min-width: 60px;
  padding: 0 8px;
  text-transform: none;
}

@media (min-width: 768px) {
  .mobile-nav {
    display: none;
  }

}

@media (max-width: 768px) {
  .logo-text {
    font-size: 0.55rem;
    line-height: 0.7rem;
  }

  .location-link {
    display: none;
  }

  .info-section {
    font-size: 0.5rem;
    line-height: 0.7rem;
    border-right: none !important;
    padding-right: 0 !important;
    margin-right: 0 !important;
  }
}
</style>
