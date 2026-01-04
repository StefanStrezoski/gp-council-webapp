<script setup>

import BaseContainer from "@/components/BaseContainer.vue";
import BaseCard from "@/components/BaseCard.vue";
import BaseList from "@/components/BaseList.vue";
import SmallCard from "@/components/SmallCard.vue";
import ParagraphNoIndent from "@/components/ParagraphNoIndent.vue";
import hotelOne from '@/assets/Hotel 1.png'
import hotelTwo from '@/assets/Hotel 2.png'
import hotelThree from '@/assets/Hotel 3.png'
import BaseParagraph from "@/components/BaseParagraph.vue";
import brandLogo from "@/assets/LogoGNA.png"
import {useI18n} from "vue-i18n";

const { t } = useI18n();
</script>

<template>
  <base-container>
    <base-card>
      <paragraph-no-indent class="text-center">
        <b><span class="text-orange-darken-3">СМЕСТУВАЊЕ</span></b>
      </paragraph-no-indent>
      <v-card rounded="l" class="mb-2 pa-1 text-center bg-light-blue-darken-4">
        <v-card-text><b>Цени за сместување во хотелот:</b></v-card-text>
      </v-card>
      <v-card outlined class="mt-5 mb-5">
        <v-responsive>
          <v-table>
            <thead>
            <tr>
              <th class="text-center bg-light-blue-darken-4 border">ХОТЕЛ</th>
              <th class="text-center bg-light-blue-darken-4 border">СМЕСТУВАЊЕ</th>
              <th class="text-center bg-light-blue-darken-4 border">ЦЕНА ПО ЛИЦЕ/НОЌ за ПОЛУПАНСИОН</th>
            </tr>
            </thead>
            <tbody>
            <tr>
              <th rowspan="2" class="bg-light-blue-lighten-4 border text-center">Хотел ДРИМ &#11088;&#11088;&#11088;&#11088; <br/> Струга </th>
              <td class="bg-light-blue-lighten-4 border">Еднокреветна</td>
              <td class="bg-light-blue-lighten-4 border"><b>80 еур</b></td>
            </tr>
            <tr>
              <td class="bg-light-blue-lighten-4 border">Двокреветна</td>
              <td class="bg-light-blue-lighten-4 border"><b>65 еур</b></td>
            </tr>
            </tbody>
          </v-table>
        </v-responsive>
      </v-card>
      <paragraph-no-indent class="text-center">
        <b>Можно е сместување и во повеќе креветни соби.</b>
      </paragraph-no-indent>
      <small-card class="bg-light-blue-darken-4 mb-5">
        <paragraph-no-indent class="text-center"><span class="text-orange-darken-3"><b>ОТКАЖУВАЊЕ НА СМЕСТУВАЊЕ</b></span></paragraph-no-indent>
        <base-list class="font-weight-bold">
          <li>За откажано сместување  <span class="text-orange-darken-3">до 16.03.2026, </span>средствата <span class="text-orange-darken-3">ќе се рефундираат во целост</span> намалени за банкарските трошоци.</li>
          <li>За откажано сместување <span class="text-orange-darken-3">од 17.03.2026, </span>средствата <span class="text-orange-darken-3">ќе се рефундираат до 50%</span> од уплатените средства, намалени за банкарските трошоци.</li>
          <li>За откажано сместување <span class="text-orange-darken-3">после 01.04.2026, НЕ СЛЕДУВА РЕФУНДАЦИЈА НА УПЛАТЕНИТЕ СРЕДСТВА.</span></li>
        </base-list>
      </small-card>
      <paragraph-no-indent class="text-center">
        <b>ПРИСУСТВО ДО СИТЕ ЛОКАЦИИ ВО ХОТЕЛ ДРИМ, ЌЕ БИДЕ ДОЗВОЛЕН
          САМО ЗА ЛИЦАТА КОИ ИМААТ ПЛАТЕНО КОТИЗАЦИЈА ЗА СИМПОЗИУМОТ.
        </b>
      </paragraph-no-indent>
      <paragraph-no-indent class="mb-5">
        <b>За сите информации и детали за сместување, превоз, техничка и логистичка поддршка, ве молиме обратете се до агенцијата организатор и реализатор:</b>
      </paragraph-no-indent>
      <div class="d-inline-block align-center">
        <v-img :src="brandLogo" :width="'10rem'"></v-img>
      </div>
      <small-card class="bg-light-blue-darken-4" elevation="0">
        <v-row dense no-gutters>
          <v-col cols="6">
            <v-row dense no-gutters justify="center" align="center">
              <v-icon class="pa-7 responsive-icon" color="white">mdi-phone-in-talk</v-icon>
              <ul class="font-weight-bold text-left custom-list" style="list-style-type: none">
                <li><a href="tel:071/317-377" class="text-white text-decoration-none">071/317-377</a>, Марјан Димески</li>
                <li><a href="tel:070/392-638" class="text-white text-decoration-none">070/392-638</a>, Лара</li>
              </ul>
            </v-row>
          </v-col>
          <v-col cols="6">
            <v-row dense no-gutters justify="center" align="center">
              <v-icon class="responsive-icon pa-7" color="white">mdi-email-outline</v-icon>
              <ul class="font-weight-bold text-left custom-list" style="list-style-type: none">
                <li><a href="mailto:globalneta@gmail.com" class="text-white">globalneta@gmail.com</a> и/или</li>
                <li><a href="mailto:globalnetadv@globalnetadv.mk" class="text-white">globalnetadv@globalnetadv.mk</a></li>
              </ul>
            </v-row>
          </v-col>
        </v-row>
        <v-row dense no-gutters>
          <v-col cols="12" class="d-inline-flex justify-center align-center">
              <v-icon class="responsive-icon pa-7" color="white">mdi-web</v-icon>
              <paragraph-no-indent class="text-center font-weight-bold">
                <a href="https://globalnetadv.mk/" target="_blank" class="text-white">www.globalnetadv.mk/</a>
              </paragraph-no-indent>
          </v-col>
        </v-row>
      </small-card>
      <v-row class="mt-5" justify="center" align="center">
        <v-col cols="4">
          <v-img :src="hotelOne" alt="hotel 1"></v-img>
        </v-col>
        <v-col cols="4">
          <v-img :src="hotelTwo" alt="hotel 2"></v-img>
        </v-col>
        <v-col cols="4">
          <v-img :src="hotelThree" alt="hotel 3"></v-img>
        </v-col>
      </v-row>
      <base-paragraph class="text-center mb-0">
        <a href="https://zplrm2026.mk/" target="_blank" class="text-decoration-none text-lg-h4 font-weight-bold"><span class="text-orange-darken-3">www.zplrm2026.mk</span></a>
      </base-paragraph>
    </base-card>
  </base-container>
</template>

<style scoped>
.title-card {
  background-color: #125280;
  color: white;
}
.responsive-icon {
  font-size: 2.5rem;
}
@media (max-width: 768px) {
  .responsive-icon {
    font-size: 1.5rem;
  }
}
.custom-list {
  font-size: 1.2rem;
  text-align: left;
  list-style-position: inside;
}

@media (max-width: 768px) {
  .custom-list {
    font-size: 0.7rem;
    text-align: left;
    list-style-position: inside;
  }
}
</style>
