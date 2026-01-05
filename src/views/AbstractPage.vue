<script setup>
/* eslint-disable */
import { ref } from "vue";
import BaseContainer from "@/components/BaseContainer.vue";
import BaseCard from "@/components/BaseCard.vue";
import BaseParagraph from "@/components/BaseParagraph.vue";
import BaseList from "@/components/BaseList.vue";
import SmallCard from "@/components/SmallCard.vue";
import { v4 as uuidv4 } from 'uuid';
import { useI18n } from 'vue-i18n';
// import { supabase } from '@/supabase/supabase.js';

const { t } = useI18n();

const formRef = ref(null);

const form = ref({
  name: '',
  email: '',
  institution: '',
  title: ''
});

const file = ref(null);
const message = ref('');
const success = ref(false);
const loading = ref(false);

const validRule = [value => !!value || t('required')];

// async function handleSubmit() {
//   try {
//     message.value = '';
//     success.value = false;
//     loading.value = true;
//
//     // 1️⃣ Validate form
//     if (!formRef.value) {
//       message.value = 'Form reference is not available.';
//       loading.value = false;
//       return;
//     }
//
//     const isValid = await formRef.value.validate();
//     if (!isValid.valid) {
//       message.value = t('errorMsg');
//       loading.value = false;
//       return;
//     }
//
//     // 2️⃣ Check file
//     if (!file.value) {
//       message.value = t('selectFile');
//       loading.value = false;
//       return;
//     }
//
//     const fileExtension = file.value.name.match(/\.[^.]+$/)[0];
//     const uniqueFileName = `${Date.now()}_${uuidv4()}${fileExtension}`;
//
//     // 3️⃣ Prepare FormData for Netlify Function
//     const formData = new FormData();
//     formData.append('name', form.value.name);
//     formData.append('email', form.value.email);
//     formData.append('institution', form.value.institution);
//     formData.append('title', form.value.title);
//     formData.append('file', file.value, uniqueFileName);
//
//     // 4️⃣ Call Netlify Function
//     const res = await fetch('/api/submit-abstracts', {
//       method: 'POST',
//       body: formData,
//     });
//
//     if (!res.ok) {
//       const errData = await res.json().catch(() => ({}));
//       message.value = errData.error || t('abstractPage.unexpectedError');
//       loading.value = false;
//       return;
//     }
//
//     // 5️⃣ Success
//     message.value = t('abstractPage.successMsg');
//     success.value = true;
//     loading.value = false;
//
//     // Reset form
//     form.value = { name: '', email: '', institution: '', title: '' };
//     file.value = null;
//     formRef.value?.resetValidation();
//     formRef.value?.reset();
//
//   } catch (error) {
//     console.error(error);
//     message.value = t('abstractPage.unexpectedError');
//     loading.value = false;
//   }
// }
</script>

<template>
  <base-container>
    <v-card rounded="xl" class="mb-2 pa-1 text-center title-card">
      <v-card-text class="text-h5 font-weight-bold">ПОДНЕСУВАЊЕ НА АПСТРАКТИ</v-card-text>
    </v-card>
    <base-card>
      <base-paragraph>
        Поднесувањетo на апстрактите се врши електронски со праќање на мејл до <a href="mailto:gnaapstrakti2@gmail.com" class="text-decoration-none">gnaapstrakti2@gmail.com</a>, со користење на прикачениот образец од веб страната:
      </base-paragraph>
      <base-paragraph class="text-center text-h4">
        <b><a href="https://zplrm2026.mk" class="text-decoration-none" target="_blank">www.zplrm2026.mk</a></b>
      </base-paragraph>
      <base-paragraph>
        Апстрактите треба да бидат напишани во фонт <b>Times New Roman</b> со <b>12</b> големина на букви, со <b>единечен проред</b> и во <b>А4 формат.</b>
      </base-paragraph>
      <base-paragraph>
        Апстрактот треба да содржи: <b>наслов, имиња и презимиња на авторите</b> (да се подвлече носителот на трудот), <b>Институција од која доаѓаат,</b>а содржината на апстрактот да биде <b>мин. 200 и макс. 300 зборови со најмногу 5 клучни зборови.</b>
      </base-paragraph>
      <base-paragraph>
        Апстрактите треба да бидат што е можно поконцизни и истите е
        препорачливо да бидат конципирани со воведен дел и цели, методологија (доколку е применлива),
        резултати и дискусија, заклучок и литература (најмногу две цитирани референци, <b>Harvard style</b>)
      </base-paragraph>
      <base-paragraph>
        <b>Апстрактите треба да бидат напишани на македонски јазик.</b>
      </base-paragraph>
      <base-paragraph>
        <b>Во прилог е пример за правилно поднесен апстрактите (<a href="/files/AbsTemp.docx" download>линк до примерот</a>).</b>
      </base-paragraph>
      <base-paragraph>
        При поднесување на апстрактите, авторот треба да избере дали сака усна или постер презентација.
      </base-paragraph>
      <base-paragraph>
        По рецензирање на апстрактите, <b>Научниот одбор го задржува правото одреден апстракт да го префрли од усна во постер презентација.</b>
      </base-paragraph>
      <base-paragraph>
        Усните излагања може да се на мајчин јазик, но презентациите <b>МОРА ДА БИДАТ НА МАКЕДОНСКИ ЈАЗИК</b> - напишани во Пауер Поинт (power point), во 16:9 формат).
      </base-paragraph>
      <base-paragraph>
        Постер презентациите ќе се изведуваат исклучиво електронски на LCD екрани и затоа истите треба да бидат направени во размер <b>16:9 и резолуција мин. 300 dpi (.pdf формат).</b>
        За сите прифатени <b>АПСТРАКТИ</b>, одлука ќе донесе Научниот одбор.
      </base-paragraph>
      <base-list class="text-center mt-10 mb-10" style="color: #134b7a">
        <li><b>КРАЕН РОК ЗА ПОДНЕСУВАЊЕ НА АПСТРАКТИТЕ: <span class="text-red">05.03.2026 год.</span></b></li>
        <li><b> КРАЕН РОК ЗА ПОТВРДА НА ПРИФАТЕНИТЕ АПСТРАКТИ: <span class="text-red">10.03.2026 год.</span></b></li>
      </base-list>
      <div class="mt-10 mb-10" style="border: 3px solid red; border-radius: 15px;">
        <base-paragraph class="text-center">
          <b><span class="text-red">НАПОМЕНА:</span>Прифатените усни и постер презентации, овозможуваат соодветни бодови од акредитацијата на настанот, <span class="text-decoration-underline">НО САМО ЗА НОСИТЕЛОТ НА ТРУДОТ</span></b>
        </base-paragraph>
        <base-paragraph class="text-center text-red">
          <b>Во Книгата на апстракти ќе бидат објавени Само прифатените апстракти</b> <br/>
          <b>ЗА КОИ НОСИТЕЛОТ НА ТРУДОТ ИМА ПЛАТЕНА/ОБЕЗБЕДЕНА КОТИЗАЦИЈА.</b>
        </base-paragraph>
      </div>
<!--      <base-paragraph class="d-inline-block align-center">-->
<!--        <v-btn-->
<!--          :href="t('abstractPage.filePath')"-->
<!--          download-->
<!--          prepend-icon="mdi-download"-->
<!--          color="primary"-->
<!--          variant="outlined"-->
<!--        >-->
<!--          {{ t('abstractPage.p13') }}-->
<!--        </v-btn>-->
<!--      </base-paragraph>-->
    </base-card>
<!--    <small-card class="mt-10 text-center">-->
<!--      <v-form @submit.prevent="handleSubmit" ref="formRef">-->
<!--        <v-text-field variant="outlined" density="comfortable" v-model="form.name" :label="t('abstractPage.nameLabel')" required :rules="validRule"/>-->
<!--        <v-text-field variant="outlined" density="comfortable" v-model="form.email" :label="t('abstractPage.emailLabel')" required type="email" :rules="validRule"/>-->
<!--        <v-text-field variant="outlined" density="comfortable" v-model="form.institution" :label="t('abstractPage.institutionLabel')" required :rules="validRule"/>-->
<!--        <v-text-field variant="outlined" density="comfortable" v-model="form.title" :label="t('abstractPage.titleLabel')" required :rules="validRule"/>-->

<!--        <v-file-input-->
<!--          v-model="file"-->
<!--          :label="t('abstractPage.uploadLabel')"-->
<!--          accept=".doc,.docx"-->
<!--          required-->
<!--          :rules="validRule"-->
<!--          variant="outlined" density="comfortable"-->
<!--          show-size-->
<!--        />-->

<!--        <v-btn type="submit" color="primary" class="mt-4" :loading="loading">-->
<!--          {{ t('abstractPage.submitLabel') }}-->
<!--        </v-btn>-->
<!--        <v-alert-->
<!--          v-if="message"-->
<!--          class="mt-4"-->
<!--          :type="success ? 'success' : 'error'"-->
<!--          border="start"-->
<!--          variant="tonal"-->
<!--        >-->
<!--          {{ message }}-->
<!--        </v-alert>-->
<!--      </v-form>-->
<!--    </small-card>-->
  </base-container>
</template>

<style scoped>
.title-card {
  background-color: #125280;
  color: white;
}
</style>
