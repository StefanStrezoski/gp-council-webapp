<script setup>
import BaseContainer from "@/components/BaseContainer.vue";
import SmallCard from "@/components/SmallCard.vue";
import BaseCard from "@/components/BaseCard.vue";
import BaseParagraph from "@/components/BaseParagraph.vue";
import ParagraphNoIndent from "@/components/ParagraphNoIndent.vue";
import BaseList from "@/components/BaseList.vue";
import { ref } from "vue";


const embedCode = ref(`
<iframe style="width:100%;height:600px" src="https://online.anyflip.com/ujmxk/pkib/index.html"  seamless="seamless" scrolling="no" frameborder="0" allowtransparency="true" allowfullscreen="true" ></iframe>`)

// const programData = {
//   title: "РАБОТНА ПРОГРАМА",
//   days: [
//     {
//       date: "ПЕТОК, 17 АПРИЛ 2026",
//       groups: [
//         {
//           title: "СТРУЧНИ СОСТАНОЦИ",
//           items: [
//             "Стручен состанок 1 ЕКГ ИНТЕРПРЕТАЦИЈА",
//             "Стручен состанок 2 ИНТРААРТИКУЛАРНА АПЛИКАЦИЈА НА ЛЕКОВИ",
//             "Стручен состанок 3 УЛТРАСОНОГРАФСКО ИСЛЕДУВАЊЕ",
//             "Стручен состанок 4 УРГЕНТНИ СОСТОЈБИ ПРИ ПОЖАРИ И ИЗГОРЕНИЦИ"
//           ]
//         },
//         {
//           title: "СТРУЧНИ СЕСИИ",
//           items: [
//             "Сесија 1 ЕНДОКРИНОЛОСКА СЕСИЈА - ДИЈАБЕТЕС МЕЛИТУС"
//           ]
//         }
//       ]
//     },
//     {
//       date: "САБОТА, 18 АПРИЛ 2026",
//       groups: [
//         {
//           items: [
//             "Сесија 2 НЕВРОЛОШКА СЕСИЈА",
//             "Сесија 3 ЕНТЕРАЛНА ИСХРАНА",
//             "Сесија 4 ПУЛМОЛОШКА СЕСИЈА",
//             "Сесија 5 ПРЕКЛАМПСИИ",
//             "Сесија 6 ПЕДИЈАТРИСКА СЕСИЈА"
//           ]
//         }
//       ]
//     },
//     {
//       date: "НЕДЕЛА, 19 АПРИЛ 2026",
//       groups: [
//         {
//           items: [
//             "Сесија 7 ТЕЛЕМЕДИЦИНА",
//             "ТРКАЛЕЗНА МАСА ЗА РАЗМЕНА НА СТАВОВИ И ДЕБАТА",
//             "„ПРЕДИЗВИЦИТЕ СО КОИ СЕ СООЧУВААТ ПРИВАТНИТЕ ЛЕКАРИ И НИВНИТЕ ОРДИНАЦИИ, СО ПРЕДЛОЗИ ЗА ВИЗИИ ЗА ПОДОБРО ЗДРАВЈЕ И ПОКВАЛИТЕТНА УСЛУГА ЗА ПАЦИЕНТИТЕ”"
//           ]
//         },
//         {
//           title: "ПОСТЕР СЕСИЈА",
//           items: [
//             "ПОСТЕР ПРЕЗЕНТАЦИИ"
//           ]
//         }
//       ]
//     }
//   ]
// };
</script>

<template>
  <base-container>
      <small-card>
      <paragraph-no-indent class="text-center"><span class="text-red"><b>ВАЖНО</b></span></paragraph-no-indent>
      <base-list class="text-red" style="font-weight: bolder;">
        <li>
          Стручните состаноци се Акредитирани од страна на Лекарската Комора на РСM,
          и тоа со 4 бодови за слушатели и 6 бодови за предавачите.
        </li>
        <li>

          Симпозиумот е Акредитирани од страна на Лекарската Комора на РСM,
          и тоа со 8 бодови за слушатели и 12 бодови за предавачите.
        </li>
      </base-list>
    </small-card>

    <small-card class="mt-5">
      <div v-html="embedCode"></div>
    </small-card>



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
