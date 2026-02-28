<script setup>
import BaseContainer from '@/components/BaseContainer.vue';
import BaseCard from '@/components/BaseCard.vue';
</script>

<template>
  <base-container>
    <v-card rounded="xl" class="mb-2 pa-1 text-center title-card">
      <v-card-text class="text-h5 font-weight-bold">Зачленување</v-card-text>
    </v-card>
    <base-card>
      <base-paragraph>
        <a href="http://registrations.zplrm.org.mk/register" target="_blank" class="text-decoration-none">Линк за
          зачленување</a>
      </base-paragraph>
    </base-card>
  </base-container>
</template>

<style scoped>
.title-card {
  background-color: #125280;
  color: white;
}
</style>
