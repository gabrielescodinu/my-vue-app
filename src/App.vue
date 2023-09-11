<template>
  <div id="app">
    <ImagesGallery />
  </div>
</template>


<script>
import ImagesGallery from './components/ImagesGallery.vue'

export default {
  name: 'App',
  components: {
    ImagesGallery,
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
}
</style>
