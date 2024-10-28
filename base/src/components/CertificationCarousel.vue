<template>
  <div class="q-ma-md">
    <q-carousel
      v-model="slide"
      transition-prev="slide-right"
      transition-next="slide-left"
      animated
      control-color="primary"
      class="rounded-borders">
      <q-carousel-slide name="Kubernetes" class="flex flex-center">
	<q-btn
	  v-for="cert in visCerts"
	  :key = cert.name
	  flat
	  dense
	  :href="cert.url"
	  target="_blank"
	  >
	  <q-img
	    class="q-mx-auto"
	    :alt="cert.name"
	    :src="'/src/assets/' + cert.img"
	    style="width: 140px; height: 140px"
	    >
	  </q-img>
	  <q-tooltip>
            {{ cert.name }}
          </q-tooltip>
	</q-btn>
      </q-carousel-slide>
      <q-carousel-slide name="SUSE" class="flex flex-center">
	<q-btn
	  v-for="cert in visCerts"
	  :key = cert.name
	  flat
	  dense
	  :href="cert.url"
	  target="_blank"
	  >
	  <q-img
	    class="q-mx-auto"
	    :alt="cert.name"
	    :src="'/src/assets/' + cert.img"
	    style="width: 140px; height: 140px"
	    >
	    <q-tooltip>
              {{ cert.name }}
            </q-tooltip>
	  </q-img>
	</q-btn>
      </q-carousel-slide>
      <q-carousel-slide name="Security" class="flex flex-center">
	<q-btn
	  v-for="cert in visCerts"
	  :key = cert.name
	  flat
	  dense
	  :href="cert.url"
	  target="_blank"
	  >
	  <q-img
	    class="q-mx-auto"
	    :alt="cert.name"
	    :src="'/src/assets/' + cert.img"
	    style="width: 140px; height: 140px"
	    >
	    <q-tooltip>
              {{ cert.name }}
            </q-tooltip>
	  </q-img>
	</q-btn>	
      </q-carousel-slide>
      <q-carousel-slide name="All" class="flex flex-center">
	<q-btn
	  v-for="cert in visCerts"
	  :key = cert.name
	  flat
	  dense
	  :href="cert.url"
	  target="_blank"
	  >
	  <q-img
	    class="q-mx-auto"
	    :alt="cert.name"
	    :src="'/src/assets/' + cert.img"
	    style="width: 140px; height: 140px"
	    >
	    <q-tooltip>
              {{ cert.name }}
            </q-tooltip>
	  </q-img>
	</q-btn>	
      </q-carousel-slide>
    </q-carousel>
    <div class="row justify-center">
      <q-btn-toggle
	v-model="slide"
	:options="slideOptions"
	/>
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'
import certData from 'assets/certs.json'

defineOptions({
  name: 'CertificationCarousel',
});

const slide = ref('All');

const slideOptions = ref([
  { label: 'All', value: 'All' },
  { label: 'Kubernetes', value: 'Kubernetes' },
  { label: 'SUSE', value: 'SUSE' },
  { label: 'Security', value: 'Security' }
])

const visCerts = computed(() => {
  let certs = []
  if (slide.value === "All")
    certs = certData["certifications"]
  else {
    for (let cert of certData["certifications"]) {
      if (cert.tags.includes(slide.value)) {
	certs.push(cert)
      }
    }
  }
  return certs
})

</script>
