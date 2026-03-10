<script setup>
import BaseContainer from "@/components/BaseContainer.vue";
import SmallCard from "@/components/SmallCard.vue";
import BaseParagraph from "@/components/BaseParagraph.vue";

const programData = {
  title: "РАБОТНА ПРОГРАМА",
  days: [
    {
      date: "ПЕТОК, 17 АПРИЛ 2026",
      groups: [
        {
          title: "СТРУЧНИ СОСТАНОЦИ",
          items: [
            "Стручен состанок 1 ЕКГ ИНТЕРПРЕТАЦИЈА",
            "Стручен состанок 2 ИНТРААРТИКУЛАРНА АПЛИКАЦИЈА НА ЛЕКОВИ",
            "Стручен состанок 3 УЛТРАСОНОГРАФСКО ИСЛЕДУВАЊЕ",
            "Стручен состанок 4 УРГЕНТНИ СОСТОЈБИ ПРИ ПОЖАРИ И ИЗГОРЕНИЦИ"
          ]
        },
        {
          title: "СТРУЧНИ СЕСИИ",
          items: [
            "Сесија 1 ЕНДОКРИНОЛОСКА СЕСИЈА - ДИЈАБЕТЕС МЕЛИТУС"
          ]
        }
      ]
    },
    {
      date: "САБОТА, 18 АПРИЛ 2026",
      groups: [
        {
          items: [
            "Сесија 2 НЕВРОЛОШКА СЕСИЈА",
            "Сесија 3 ЕНТЕРАЛНА ИСХРАНА",
            "Сесија 4 ПУЛМОЛОШКА СЕСИЈА",
            "Сесија 5 ПРЕКЛАМПСИИ",
            "Сесија 6 ПЕДИЈАТРИСКА СЕСИЈА"
          ]
        }
      ]
    },
    {
      date: "НЕДЕЛА, 19 АПРИЛ 2026",
      groups: [
        {
          items: [
            "Сесија 7 ТЕЛЕМЕДИЦИНА",
            "ТРКАЛЕЗНА МАСА ЗА РАЗМЕНА НА СТАВОВИ И ДЕБАТА",
            "„ПРЕДИЗВИЦИТЕ СО КОИ СЕ СООЧУВААТ ПРИВАТНИТЕ ЛЕКАРИ И НИВНИТЕ ОРДИНАЦИИ, СО ПРЕДЛОЗИ ЗА ВИЗИИ ЗА ПОДОБРО ЗДРАВЈЕ И ПОКВАЛИТЕТНА УСЛУГА ЗА ПАЦИЕНТИТЕ”"
          ]
        },
        {
          title: "ПОСТЕР СЕСИЈА",
          items: [
            "ПОСТЕР ПРЕЗЕНТАЦИИ"
          ]
        }
      ]
    }
  ]
};
</script>

<template>
  <base-container>
    <!-- Title Section -->
    <small-card class="mt-10">
      <base-paragraph class="hero-title text-center">
        <b>{{ programData.title }}</b>
      </base-paragraph>
    </small-card>

    <div class="mt-10">
      <!-- Day Sections -->
      <div v-for="(day, dayIndex) in programData.days" :key="dayIndex" class="mb-8">
        <small-card>
          <h3 class="program-date mb-4">{{ day.date }}</h3>

          <div v-for="(group, groupIndex) in day.groups" :key="groupIndex" class="group-wrapper mb-6">
            <div v-if="group.title" class="hall-info mb-2"><b>{{ group.title }}</b></div>
            <v-list density="compact" class="program-list">
              <v-list-item v-for="(item, itemIndex) in group.items" :key="itemIndex" class="px-0">
                <div class="d-flex align-start">
                  <v-icon size="small" color="primary" class="mr-3 mt-1">mdi-circle-medium</v-icon>
                  <span class="program-item-text">{{ item }}</span>
                </div>
              </v-list-item>
            </v-list>
          </div>
        </small-card>
      </div>
    </div>
  </base-container>
</template>

<style scoped>
.hero-title {
  font-size: 2rem;
  color: #004679;
  margin: 0;
  padding: 10px 0;
}

.program-date {
  color: #207cbd;
  font-size: 1.5rem;
  font-weight: 700;
  border-bottom: 2px solid #e0e0e0;
  padding-bottom: 8px;
}

.hall-info {
  color: #004679;
  font-size: 1.1rem;
  text-transform: uppercase;
}

.program-list {
  background: transparent !important;
}

.program-item-text {
  color: #333;
  line-height: 1.6;
  font-size: 1.1rem;
  font-weight: 500;
}

:deep(.v-list-item__content) {
  padding: 4px 0;
}

@media (max-width: 768px) {
  .hero-title {
    font-size: 1.4rem;
  }

  .program-date {
    font-size: 1.1rem;
  }

  .program-item-text {
    font-size: 0.95rem;
  }
}
</style>
