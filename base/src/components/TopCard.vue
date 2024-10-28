<template>
  <q-card
    class="shadow-7 q-ma-xl"
    style="background: #fafafa"
    >
    <q-card-section horizontal
      class="q-ma-lg"
      style="max-width: 650pt">
      <div class="row">
	<div class="col-6 col-md-6 col-sm-12 col-xs-12 column justify-evenly q-my-md">
	  <p class="col-4 text-h3 q-mb-md">Linus Östberg</p>
	  <p class="col-4 text-h5">Full-stack developer and Kubernetes specialist with a passion for security</p>
	</div>
	<div class="col-6 col-md-6 col-xs-12 flex flex-center">
	  <q-btn
	    flat
	    dense
	    href="https://www.credly.com/badges/e9fc7a24-6dd2-4550-bfa8-c173b40d405a/public_url"
	    target="_blank">
	    <q-img
	      class="q-mx-auto"
	      alt="Kubestronaut"
	      src="~assets/kubestronaut.png"
	      style="width: 175px; height: 175px"
	      >
	    </q-img>
	  </q-btn>
	  <q-btn
	    flat
	    dense
	    href="https://www.credly.com/badges/c31a5413-2d14-4474-bdf9-cc66cd44c19d/public_url"
	    target="_blank">
	    <q-img
	      class="q-mx-auto"
	      alt="Certified in Cybersecurity"
	      src="~assets/cc.png"
	      style="width: 175px; height: 175px"
	      >
	    </q-img>
	  </q-btn>
	</div>
      </div>
      <q-card-actions
	vertical
	class="justify-center">
	<q-btn
	  flat
	  round
	  href="https://github.com/talavis"
	  target="_blank"
	  color="primary"
	  :icon="fabGithub" />
	<q-btn
	  flat
	  round
	  href="https://www.linkedin.com/in/linusostberg/"
	  target="_blank"
	  color="primary"
	  :icon="fabLinkedin" />
	<q-btn
	  flat
	  round
	  href="https://scholar.google.com/citations?user=nafzIpQAAAAJ"
	  target="_blank"
	  color="primary"
	  :icon="fasBookOpen" />
      </q-card-actions>
    </q-card-section>
  </q-card>
</template>

<script setup>
import { fasBookOpen, fabLinkedin, fabGithub } from '@quasar/extras/fontawesome-v6'

defineOptions({
  name: 'TopCard',
});
</script>
