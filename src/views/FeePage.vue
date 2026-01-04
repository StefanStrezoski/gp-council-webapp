<script setup>
/* eslint-disable */
import BaseContainer from "@/components/BaseContainer.vue";
import BaseCard from "@/components/BaseCard.vue";
import SmallCard from "@/components/SmallCard.vue";
import BaseList from "@/components/BaseList.vue";
import ParagraphNoIndent from "@/components/ParagraphNoIndent.vue";
import {useI18n} from "vue-i18n";
import {ref} from "vue";
import {v4 as uuidv4} from "uuid";
import BaseParagraph from "@/components/BaseParagraph.vue";
import {supabase} from "@/supabase/supabase.js";

const { t } = useI18n();

const formRef = ref(null);
const form = ref({
  name: '',
  email: '',
  phone: '',
  institution: '',
  category: null,
});
const file = ref(null);
const message = ref('');
const success = ref(false);
const loading = ref(false);
const hide = ref(false);

const categoryOptions = [
  { title: '1', value: 1 },
  { title: '2', value: 2 },
  { title: '3', value: 3 },
  { title: '4', value: 4 },
  { title: '5', value: 5 },
];

const validRule = [value => !!value || t('required')];
const phoneRule = [
  value => !!value || t('feesPage.required'),
  value => /^[0-9+()-]{8,}$/.test(value) || t('feesPage.invalidPhone'),
];
const fileRule = [
  () => form.value.category !== 4 || !!file.value || t('feesPage.selectFile'),
];

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
      message.value = t('feesPage.errorMsg');
      loading.value = false;
      return;
    }

    let fileName = null;

    if (form.value.category === 4) {
      if (!file.value) {
        message.value = t('feesPage.selectFile');
        loading.value = false;
        return;
      }

      const fileExtension = file.value.name.match(/\.[^.]+$/)[0].toLowerCase();
      fileName = `documents/${Date.now()}_${uuidv4()}${fileExtension}`;

      const { error: uploadError } = await supabase.storage
        .from('abstracts')
        .upload(fileName, file.value);

      if (uploadError) {
        message.value = `File upload failed: ${uploadError.message}`;
        loading.value = false;
        return;
      }

    }

    const { error: dbError } = await supabase.from('document_submissions').insert({
      name: form.value.name,
      email: form.value.email,
      phone: form.value.phone,
      institution: form.value.institution,
      category: form.value.category,
      file_name: fileName,
    });

    if (dbError) {
      message.value = `Failed to save submission: ${dbError.message}`;
      loading.value = false;
      return;
    }

    message.value = t('feesPage.successMsg') || 'Submission successful!';
    success.value = true;
    loading.value = false;

    form.value = { name: '', email: '', phone: '', institution: '', category: null };
    file.value = null;

    formRef.value?.resetValidation();
    formRef.value?.reset();
  } catch (error) {
    message.value = t('feesPage.unexpectedError') || 'An unexpected error occurred.';
    loading.value = false;
  }
}
</script>

<template>
  <base-container>
    <base-card>
      <paragraph-no-indent class="text-center">
        <b><span class="text-orange-darken-3">КОТИЗАЦИЈА ЗА УЧЕСНИЦИ</span></b>
      </paragraph-no-indent>
      <v-card outlined class="mt-5 mb-5">
        <v-responsive>
          <v-table class="elevation-1">
            <thead>
            <tr>
              <th class="text-center bg-light-blue-darken-2 border">
                Категорија
              </th>
              <th class="text-center bg-light-blue-darken-2 border">
                Вид на учесник
              </th>
              <th class="text-center bg-light-blue-darken-2 border">
                УПЛАТА
                ДО 16.03.2026
              </th>
              <th class="text-center bg-light-blue-darken-2 border">
                УПЛАТА
                ОД 17.03.2026
              </th>
              <th class="text-center bg-light-blue-darken-2 border">
                УПЛАТА
                ОД 01.04.2026
              </th>
            </tr>
            </thead>
            <tbody>
            <tr>
              <td class="bg-light-blue-lighten-4 border">
                1
              </td>
              <td class="bg-light-blue-lighten-4 border">
                Членови на ЗПЛРМ
              </td>
              <td class="bg-light-blue-lighten-4 border">
                8.990 ден. <br/>
                145 € <br/>
                (123 € + 18% ДДВ)
              </td>
              <td class="bg-light-blue-lighten-4 border">
                9.920 ден. <br/>
                160 € <br/>
                (136 € + 18% ДДВ)
              </td>
              <td class="bg-light-blue-lighten-4 border">
                11.160 ден. <br/>
                180 € <br/>
                (153 € + 18% ДДВ)
              </td>
            </tr>
            <tr>
              <td class="bg-light-blue-lighten-4 border">
                2
              </td>
              <td class="bg-light-blue-lighten-4 border">
                Останати УЧЕСНИЦИ
              </td>
              <td class="bg-light-blue-lighten-4 border">
                10.230 ден. <br/>
                165 € <br/>
                (140 € + 18% ДДВ)
              </td>
              <td class="bg-light-blue-lighten-4 border">
                11.470 ден. <br/>
                185 € <br/>
                (157 € + 18% ДДВ)
              </td>
              <td class="bg-light-blue-lighten-4 border">
                12.710 ден. <br/>
                205 € <br/>
                (174 € + 18% ДДВ)
              </td>
            </tr>
            <tr>
              <td class="bg-light-blue-lighten-4 border">
                3
              </td>
              <td class="bg-light-blue-lighten-4 border">
                &#11088; Специјализанти и студенти
              </td>
              <td class="bg-light-blue-lighten-4 border">
                6.820 ден. <br/>
                110 € <br/>
                (93 € + 18% ДДВ)
              </td>
              <td class="bg-light-blue-lighten-4 border">
                8.060 ден. <br/>
                130 € <br/>
                (110 € + 18% ДДВ)
              </td>
              <td class="bg-light-blue-lighten-4 border">
                8.990 ден. <br/>
                145 € <br/>
                (123 € + 18% ДДВ)
              </td>
            </tr>
            <tr>
              <td class="bg-light-blue-lighten-4 border">
                4
              </td>
              <td class="bg-light-blue-lighten-4 border">
                &#11088;&#11088; Претставници на компании и други придружби
              </td>
              <td class="bg-light-blue-lighten-4 border">
                6.820 ден. <br/>
                110 € <br/>
                (93 € + 18% ДДВ)
              </td>
              <td class="bg-light-blue-lighten-4 border">
                8.060 ден. <br/>
                130 € <br/>
                (110 € + 18% ДДВ)
              </td>
              <td class="bg-light-blue-lighten-4 border">
                8.990 ден. <br/>
                145 € <br/>
                (123 € + 18% ДДВ)
              </td>
            </tr>
            </tbody>
          </v-table>
        </v-responsive>
      </v-card>
      <paragraph-no-indent class="text-center">
        <b><span class="text-orange-darken-3">ВО ЦЕНАТА Е ВКЛУЧЕН 18% ДДВ</span></b>
      </paragraph-no-indent>
      <paragraph-no-indent class="text-center">
        <b style="color: #125280">
          Висината на цената за секоја категорија, ќе се пресметува според
          ДАДЕНИТЕ РОКОВИ ВО ЦЕНОВНИКОТ и ДЕНОТ НА УПЛАТАТА. <br/>
          КОТИЗАЦИЈАТА ЗА СИМПОЗИУМОТ ВКЛУЧУВА:
        </b>
      </paragraph-no-indent>
      <small-card class="bg-light-blue-darken-2 mb-5">
        <v-row>
          <v-col cols="6">
            <base-list class="text-white font-weight-bold">
              <li>Комплет материјал и ID карта</li>
              <li>Присуство на сите научни сесии</li>
              <li>Сертификат со одобрени бодови од ЛКРСМ</li>
            </base-list>
          </v-col>
          <v-col cols="6">
            <base-list class="text-white font-weight-bold">
              <li>Пристап во изложбениот простор</li>
              <li>Присуство на свечено отворање</li>
              <li>Присуство на кафе паузи и организирани ручеци</li>
            </base-list>
          </v-col>
        </v-row>
        <base-paragraph class="text-center mt-5 text-white">&#11088;&#11088;&#11088;ПРИСУСТВО НА СВЕЧЕНИ ВЕЧЕРИ</base-paragraph>
      </small-card>
      <paragraph-no-indent>
        <b>
          НАПОМЕНА за категоријата &#11088;: СПЕЦИЈАЛИЗАНТИТЕ И СТУДЕНТИТЕ, ЗАДОЛЖИТЕЛНО мора да испратат Потврда од факултет дека се запишани на студии во тек во тековната учебна година. Учесниците во оваа категорија, добиваат ID картица,
          програма и слободен влез на сите предавања и други организирани активности. Овие учесници <span class="text-orange-darken-3">ДОБИВААТ СЕРТИФИКАТ ЗА УЧЕСТВО, НО НЕ ИМ СЛЕДУВААТ ПЕЧАТЕНИ МАТЕРИЈАЛИ.</span>
        </b>
      </paragraph-no-indent>
      <paragraph-no-indent>
        <b>
          НАПОМЕНА за категоријата &#11088;&#11088;: Претставници на компании може да бидат само вработени во Компании кои УЧЕСТВУВАААТ на настанот со Спонзорски пакет или со Штанд. Други придружници може да бидат само брачни другари или членови на потесно семество. Учесниците во оваа категорија,
          добиваат програма и ID картица за слободен влез на сите предавања и други организирани активности. Овие учесници  <span class="text-orange-darken-3">НЕ ДОБИВААТ СЕРТИФИКАТ ЗА УЧЕСТВО И ПЕЧАТЕНИ МАТЕРИЈАЛИ.</span>
        </b>
      </paragraph-no-indent>
      <paragraph-no-indent>
        <b>
          НАПОМЕНА за категоријата &#11088;&#11088;&#11088;: <span class="text-decoration-underline">СИТЕ УЧЕСНИЦИ КОИ ИМААТ ПЛАТЕНО КОТИЗАЦИЈА И НЕ СЕ СМЕСТЕНИ ВО ХОТЕЛ ДРИМ</span>, ЗА ПРИСУСТВО НА СВЕЧЕНИТЕ ВЕЧЕРИ, РУЧЕЦИ, КОКТЕЛИ И/ИЛИ КАФЕ ПАУЗИ, АВАНСНО ПЛАЌААТ 45 € ПО ЧОВЕК, ЗА СЕКОЈА ВЕЧЕРА ПОСЕБНО.
        </b>
      </paragraph-no-indent>
      <paragraph-no-indent>
        <b>
          <span class="text-decoration-underline">СИТЕ УЧЕСНИЦИ КОИ НЕМААТ ПЛАТЕНО КОТИЗАЦИЈА И НЕ СЕ СМЕСТЕНИ ВО ХОТЕЛ ДРИМ</span>, ЗА ПРИСУСТВО НА СВЕЧЕНИТЕ ВЕЧЕРИ, РУЧЕЦИ, КОКТЕЛИ И/ИЛИ КАФЕ ПАУЗИ, АВАНСНО ПЛАЌААТ 60 € ПО ЧОВЕК, ЗА СЕКОЈ ОБРОК ПОСЕБНО.
        </b>
      </paragraph-no-indent>
      <paragraph-no-indent class="text-center">
        <b style="color: #125280">
          Уплатата за КОТИЗАЦИЈА се врши во агенцијата ГЛОБАЛ НЕТ АДВ <br/>
          жиро сметка: 200-0031649772-96 | Стопанска банка а.д. Скопjе<br/>
          Цел на плаќање: ,,Симпозиум ЗПЛРМ 17 -19.04.2026,, <br/>
          <span class="text-orange-darken-3">Сите уплати мора да бидат завршени пред започнување на настанот!</span>
        </b>
      </paragraph-no-indent>
    </base-card>
    <small-card class="mt-10" v-if="!hide">
      <v-form @submit.prevent="handleSubmit" ref="formRef">
        <v-text-field
          variant="outlined"
          density="comfortable"
          v-model="form.name"
          label="Име"
          required
          :rules="validRule"
        />
        <v-text-field
          variant="outlined"
          density="comfortable"
          v-model="form.email"
          label="Е-пошта"
          required
          type="email"
          :rules="validRule"
        />
        <v-text-field
          variant="outlined"
          density="comfortable"
          v-model="form.phone"
          label="Телефонски број"
          required
          :rules="phoneRule"
        />
        <v-text-field
          variant="outlined"
          density="comfortable"
          v-model="form.institution"
          label="Институција"
          required
          :rules="validRule"
        />
        <v-select
          variant="outlined"
          density="comfortable"
          v-model="form.category"
          label="Категорија"
          :items="categoryOptions"
          item-title="title"
          item-value="value"
          required
          :rules="validRule"
        />
        <v-file-input
          v-if="form.category === 4"
          v-model="file"
          label="Прикачи документ (JPG, JPEG)"
          accept="image/jpeg,image/png,image/heic"
          :rules="fileRule"
          variant="outlined"
          density="comfortable"
          show-size
        />
        <v-btn type="submit" color="primary" class="mt-4" :loading="loading">
          Поднеси
        </v-btn>
        <v-alert
          v-if="message"
          class="mt-4"
          :type="success ? 'success' : 'error'"
          border="start"
          variant="tonal"
        >
          {{ message }}
        </v-alert>
      </v-form>
    </small-card>
  </base-container>
</template>

<style scoped>
</style>
