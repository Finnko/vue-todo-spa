<template>
  <div id="app">
    <div class="container">
      <Navbar />
      <router-view/>
    </div>
  </div>
</template>

<script>
  import Navbar from '@/components/Navbar';
  export default {
      components: {Navbar}
  }
</script>

<style lang="scss">
  @import "~materialize-css/dist/css/materialize.min.css";
</style>
