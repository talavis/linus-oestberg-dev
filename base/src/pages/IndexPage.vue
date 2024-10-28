<template>
  <q-page class="flex flex-center">
    <TopCard />
    <CertificationCarousel />
  </q-page>
</template>

<script setup>
import TopCard from 'components/TopCard.vue'
import CertificationCarousel from 'components/CertificationCarousel.vue'

defineOptions({
  name: 'IndexPage'
});
</script>
