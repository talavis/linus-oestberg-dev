<template>
<div id="app">
  <div class="container">
    <div class="block">
      <section class="section">
        <p class="title">Linus Östberg</p> 
      </section>
    </div>
    <div class="is-flex-direction-row is-justify-content-center">
      <a href="https://github.com/talavis"
         class="mx-4">
        <b-icon pack="fab"
                icon="github"
                size="is-large" />
      </a>

      <a href="https://www.linkedin.com/in/linusostberg/"
         class="mx-4">
        <b-icon pack="fab"
                icon="linkedin"
                size="is-large" />
      </a>

      <a href="https://scholar.google.com/citations?user=nafzIpQAAAAJ"
         class="mx-4">
        <b-icon pack="fas"
                icon="university"
                size="is-large" />
      </a>
    </div>
  </div>
</div>
</template>

<script>

export default {
  name: 'App',
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
