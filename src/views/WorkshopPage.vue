<script setup>

import BaseContainer from "@/components/BaseContainer.vue";
import SmallCard from "@/components/SmallCard.vue";
import BaseParagraph from "@/components/BaseParagraph.vue";
import { ref, onMounted, computed } from "vue";
import { supabase } from "@/supabase/supabase.js";

const formRef = ref(null);
const loading = ref(false);
const message = ref('');
const success = ref(false);

const formData = ref({
  firstName: '',
  lastName: '',
  institution: '',
  country: '',
  email: '',
  phone: '',
  category: null,
});

const SUBMISSION_LIMIT = 35;
const categoryCounts = ref({});

const categoriesData = [
  { title: 'ЕКГ ИНТЕРПРЕТАЦИЈА', value: 'EKG INTERPRETACIJA' },
  { title: 'ИНТРААРТИКУЛАРНА АПЛИКАЦИЈА НА ЛЕКОВИ', value: 'INTRAARTIKULARNA APLIKACIJA NA LEKOVI' },
  { title: 'УЛТРАСОНОГРАФСКО ИСЛЕДУВАЊЕ', value: 'ULTRASOLOGRAFSKO ISLEDUVANJE' },
  { title: 'УРГЕНТНИ СОСТОЈБИ ПРИ ПОЖАРИ И ИЗГОРЕНИЦИ', value: 'URGENTNI SOSTOJBI PRI POZARI I IZGORENICI' }
];

const categories = computed(() => {
  return categoriesData.map(cat => {
    const count = categoryCounts.value[cat.value] || 0;
    const isFull = count >= SUBMISSION_LIMIT;
    const isDisabled = cat.props?.disabled || isFull;

    let title = cat.title;
    if (isFull) {
      title += ' (ПОПОЛНЕТО)';
    }

    return {
      ...cat,
      title,
      props: { disabled: isDisabled }
    };
  });
});

async function fetchSubmissionCounts() {
  try {
    const { data, error } = await supabase
      .from('workshop_submissions_zplrm')
      .select('category');

    if (error) throw error;

    const counts = {};
    data.forEach(sub => {
      if (sub.category) {
        counts[sub.category] = (counts[sub.category] || 0) + 1;
      }
    });
    categoryCounts.value = counts;
  } catch (err) {
    console.error('Error fetching counts:', err);
  }
}

onMounted(() => {
  fetchSubmissionCounts();
});

const rules = {
  required: value => !!value || 'Задолжително поле!',
  email: value => {
    const pattern = /^(([^<>()[\]\\.,;:\s@"]+(\.[^<>()[\]\\.,;:\s@"]+)*)|(".+"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$/;
    return pattern.test(value) || 'Невалиден е-маил.';
  },
};

async function handleSubmit() {
  try {
    message.value = '';
    success.value = false;
    loading.value = true;

    if (!formRef.value) {
      message.value = 'Form reference is not available.';
      loading.value = false;
      return;
    }

    const isValid = await formRef.value.validate();
    if (!isValid.valid) {
      message.value = 'Грешка при пополнување на формуларот!';
      loading.value = false;
      return;
    }

    // Re-verify count before final submission
    const { data: currentSubs, error: countError } = await supabase
      .from('workshop_submissions_zplrm')
      .select('id', { count: 'exact', head: true })
      .eq('category', formData.value.category);

    if (countError) {
      message.value = 'Грешка при проверка на капацитет!';
      loading.value = false;
      return;
    }

    if (currentSubs >= SUBMISSION_LIMIT) {
      message.value = 'Оваа категорија штотуку се пополни! Ве молиме изберете друга.';
      await fetchSubmissionCounts(); // Refresh UI
      loading.value = false;
      return;
    }

    const { error: dbError } = await supabase.from('workshop_submissions_zplrm').insert({
      name: `${formData.value.firstName} ${formData.value.lastName}`,
      email: formData.value.email,
      institution: formData.value.institution,
      country: formData.value.country,
      phone_number: formData.value.phone,
      category: formData.value.category,
    });

    if (dbError) {
      message.value = 'Неуспешно се запишуваше! ' + dbError.message;
      loading.value = false;
      return;
    }

    message.value = 'Вашата пријава е успешно запишана!';
    success.value = true;
    loading.value = false;

    formData.value = {
      firstName: '',
      lastName: '',
      institution: '',
      country: '',
      email: '',
      phone: '',
      category: null,
    };

    formRef.value?.resetValidation();
    formRef.value?.reset();
  } catch (error) {
    message.value = 'Се појави неочекувана грешка!';
    loading.value = false;
    console.error(error);
  }
}

</script>

<template>
  <base-container>
    <!-- Title Section -->
    <small-card class="mt-10 mb-8">
      <base-paragraph class="hero-title text-center">
        <b>ПРИЈАВА ЗА АКРЕДИТИРАНИ СТРУЧНО ЕДУКАТИВНИ СОСТАНОЦИ</b>
      </base-paragraph>
    </small-card>

    <!-- Info Content Section -->
    <small-card class="pa-6 mb-8">
      <base-paragraph>
        Почитувани колеги, соработници и драги пријатели,
      </base-paragraph>
      <base-paragraph>
        Ми претставува особена чест и задоволство да ве поканам на планираните Акредитирани СТРУЧНО ЕДУКАТИВНИ
        СОСТАНОЦИ, кои се дел од Првиот Симпозиум на Здружението на приватните лекари во Р. Македонија – “ЗПЛРМ”- “КАКО
        ДО ПОДОБРО ЗДРАВЈЕ”.
      </base-paragraph>
      <base-paragraph>
        Секој СТРУЧНО ЕДУКАТИВЕН СОСТАНОК ќе биде посебно акредитиран со соодветен број на бодови добиени од Лекарската
        Комора на РС Македонија.
      </base-paragraph>
      <base-paragraph>
        Поради ограничените просторни можности и употреба на соодветна медицинска опрема за време на едукативните
        состаноци, ве молиме навремено да го пријавите вашето учество.
      </base-paragraph>
      <base-paragraph>
        Бројот на учесници на еден едукативен состанок е ограничен на максимум 30 учесници и пријави ќе се примаат до
        моментот на пополнување на одобрениот број на учесници.
      </base-paragraph>
      <base-paragraph>
        Принципот на пријава за учество на едукативните состаноци е <b>ПРВ ПРИЈАВЕН, ПРВ УСЛУЖЕН</b>.
      </base-paragraph>
      <base-paragraph>
        Врз основа на вашата првично прифатена пријава за некоја или повеќе СТРУЧНО ЕДУКАТИВНИ СОСТАНОЦИ, ќе ви биде
        доставено известување од страна на Глобал Нет Адв дека треба да ја платите Про-фактурата за Рана Котизација и за
        вкупниот број на пријавени едукативните состаноци, или само за Учество на едукативните состаноци без учество на
        Симпозиум.
      </base-paragraph>
      <base-paragraph>
        Со достава на извршената уплата, од страна на Глобал Нет Адв ќе ви биде издадена Потврда за ваше учество на
        Симпозиумот и пријавените стручно едукативни состаноци.
      </base-paragraph>

      <base-paragraph class="text-center font-weight-bold">
        ЦЕНИ ЗА УЧЕСТВО НА СТРУЧНИ СОСТАНОЦИ
      </base-paragraph>
      <v-responsive>
        <v-table class="elevation-1">
          <thead>
            <tr>
              <th class="text-center bg-light-blue-darken-2 border">
                Вид на учесник
              </th>
              <th class="text-center bg-light-blue-darken-2 border">
                ЦЕНИ НА УЧЕСТВО НА АКРЕДИТИРАН СТРУЧЕН СОСТАНОК <br /> (цените се однесуваат за Учество на 1 (еден)
                стручен состанок !
              </th>
            </tr>
          </thead>
          <tbody>
            <tr>
              <td class="cell-bg border">
                ЧЛЕНОВИ НА ЗПЛРМ Уплата за РАНА КОТИЗАЦИЈА и
              </td>
              <td class="cell-bg border">
                + 1.600 ден. <br />(25 €) </td>
            </tr>
            <tr>
              <td class="cell-bg border">
                ОСТАНАТИ УЧЕСНИЦИ Уплата за РАНА КОТИЗАЦИЈА и
              </td>
              <td class="cell-bg border">
                + 2.170 ден. <br /> (35 €)
              </td>
            </tr>
            <tr>
              <td class="cell-bg border">
                СПЕЦИЈАЛИЗАНТИ И СТУДЕНТИ Уплата за РАНА КОТИЗАЦИЈА и
              </td>
              <td class="cell-bg border">
                + 1.250 ден. <br />(20 €)
              </td>
            </tr>
            <tr>
              <td class="cell-bg border">
                УЧЕСТВО САМО НА СТРУЧЕН СОСТАНОК, БЕЗ УЧЕСТВО НА СИМПОЗИУМ
              </td>
              <td class="cell-bg border">
                3.100 ден. <br />(50€)
              </td>
            </tr>
          </tbody>
        </v-table>
      </v-responsive>
    </small-card>


    <!-- Existing Registration Form (Hidden based on 'hide' ref) -->
    <v-card v-if="hide" rounded="xl" class="mb-2 pa-1 text-center title-card">
      <v-card-text v-if="hide">
        <h3>Регистрирајте го вашето присуство на сателитскиот симпозиум</h3>
      </v-card-text>
      <v-card-text class="text-red">
        <h2>Во моментов нема активни сателитски симпозиуми!</h2>
      </v-card-text>
    </v-card>
    <small-card v-else class="mb-10">
      <base-paragraph class="text-center font-weight-bold">
        Вашето пријавување за учество на еден или повеќе стручни состаноци, потребно е да го направите тука:
      </base-paragraph>
      <v-form ref="formRef" @submit.prevent="handleSubmit">
        <v-row>
          <v-col cols="12" md="6">
            <v-text-field v-model="formData.firstName" variant="outlined" density="comfortable" clearable
              :rules="[rules.required]" required>
              <template v-slot:label>
                Име <span class="text-red">*</span>
              </template>
            </v-text-field>
          </v-col>

          <v-col cols="12" md="6">
            <v-text-field v-model="formData.lastName" variant="outlined" density="comfortable" clearable
              :rules="[rules.required]" required>
              <template v-slot:label>
                Презиме <span class="text-red">*</span>
              </template>
            </v-text-field>
          </v-col>

          <v-col cols="12">
            <v-text-field v-model="formData.institution" clearable placeholder="Внесете вашата институција"
              variant="outlined" density="comfortable" :rules="[rules.required]" required>
              <template v-slot:label>
                Институција <span class="text-red">*</span>
              </template>
            </v-text-field>
          </v-col>

          <v-col cols="12" md="6">
            <v-text-field v-model="formData.country" variant="outlined" density="comfortable" clearable
              :rules="[rules.required]" required>
              <template v-slot:label>
                Држава <span class="text-red">*</span>
              </template>
            </v-text-field>
          </v-col>

          <v-col cols="12" md="6">
            <v-text-field v-model="formData.email" type="email" variant="outlined" clearable density="comfortable"
              :rules="[rules.required, rules.email]" required>
              <template v-slot:label>
                E-mail <span class="text-red">*</span>
              </template>
            </v-text-field>
          </v-col>

          <v-col cols="12" md="6">
            <v-text-field v-model="formData.phone" variant="outlined" density="comfortable" clearable
              :rules="[rules.required]" required>
              <template v-slot:label>
                Телефонски број <span class="text-red">*</span>
              </template>
            </v-text-field>
          </v-col>

          <v-col cols="12" md="6">
            <v-select v-model="formData.category" :items="categories" variant="outlined" density="comfortable"
              :rules="[rules.required]" required>
              <template v-slot:label>
                Категорија <span class="text-red">*</span>
              </template>
            </v-select>
          </v-col>

          <v-col cols="12" class="text-center">
            <v-btn type="submit" color="primary" size="large" class="px-10" :loading="loading" :disabled="loading">
              Потврди
            </v-btn>
          </v-col>
        </v-row>
      </v-form>

      <v-alert v-if="message" :type="success ? 'success' : 'error'" variant="tonal" class="mt-6" closable>
        {{ message }}
      </v-alert>

      <base-paragraph class="mt-6">
        Уплатата за учество се врши во агенцијата ГЛОБАЛ НЕТ АДВ на
        жиро сметка: <b>200-0031649772-96</b> Стопанска банка а.д. Скопjе
        Цел на плаќање:
        <b>"Стручен симпозиум бр._____ на ЗПЛРМ 17.04.2026"</b>.
        <span class="text-red font-weight-bold">Сите уплати мора да бидат завршени пред започнување на
          настанот!</span>
      </base-paragraph>

      <base-paragraph class="mt-8 text-grey-darken-1 text-center">
        <b>НАПОМЕНА:</b> Доколку не можете да се пријавите на некој од приложените стручни состаноци, тоа значи дека
        бројката
        од
        максимално дозволените учесници е исполнета. <br /> За дополнителни информации, слободно контактирајте ја
        агенцијата
        Глобал Нет Адв, на телефонските броеви: <br /> <b>071/317-377</b> Марјан Димески или <b>070/392-638</b> Лара
        Велевска
      </base-paragraph>
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

.cell-bg {
  background-color: #c2e8ff;
  font-weight: bold;
  text-align: center;
}

@media (max-width: 768px) {
  .hero-title {
    font-size: 1.4rem;
  }
}
</style>
