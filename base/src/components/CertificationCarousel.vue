<template>
  <div class="q-ma-md">
    <div class="row justify-center">
      <q-btn-toggle
	v-model="slide"
	:options="slideOptions"
	class="q-mb-sm"
	/>
    </div>
    <q-carousel
      v-model="slide"
      transition-prev="slide-right"
      transition-next="slide-left"
      animated
      control-color="primary"
      class="rounded-borders">
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
	    :src="cert.img"
	    style="width: 140px; height: 140px"
	    >
	    <q-tooltip>
              {{ cert.name }}
            </q-tooltip>
	  </q-img>
	</q-btn>	
      </q-carousel-slide>
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
	    :src="cert.img"
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
	    :src="cert.img"
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
	    :src="cert.img"
	    style="width: 140px; height: 140px"
	    >
	    <q-tooltip>
              {{ cert.name }}
            </q-tooltip>
	  </q-img>
	</q-btn>	
      </q-carousel-slide>
    </q-carousel>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'
import certData from 'assets/certs.json'
import ccUrl from '../assets/cc.png'
import cksUrl from '../assets/cks.png'
import ckaUrl from '../assets/cka.png'
import ckadUrl from '../assets/ckad.png'
import kcnaUrl from '../assets/kcna.png'
import kcsaUrl from '../assets/kcsa.png'
import pcaUrl from '../assets/pca.png'
import psmUrl from '../assets/psm1.png'
import kubestronautUrl from '../assets/kubestronaut.png'
import scaLonghornUrl from '../assets/sca_longhorn.png'
import scdsRancherUrl from '../assets/scds_rancher.png'
import scaNeuvectorUrl from '../assets/sca_neuvector.png'

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

const certImgMap = {
  "cc.png": ccUrl,
  "cka.png": ckaUrl,
  "ckad.png": ckadUrl,
  "cks.png": cksUrl,
  "pca.png": pcaUrl,
  "kubestronaut.png": kubestronautUrl,
  "kcna.png": kcnaUrl,
  "kcsa.png": kcsaUrl,
  "psm1.png": psmUrl,
  "sca_longhorn.png": scaLonghornUrl,
  "scds_rancher.png": scdsRancherUrl,
  "scds_neuvector.png": scdsNeuvectorUrl,
  "sca_neuvector.png": scaNeuvectorUrl,
}

for (let pos in certData["certifications"]) {
  certData.certifications[pos].img = certImgMap[certData.certifications[pos].img]
}

const visCerts = computed(() => {
  let certs = []
  if (slide.value === "All")
    certs = certData.certifications
  else {
    for (let cert of certData.certifications) {
      if (cert.tags.includes(slide.value)) {
	certs.push(cert)
      }
    }
  }
  return certs
})

</script>
