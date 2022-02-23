<template>
  <div id="app">
    <Nav />
    <router-view />
  </div>
</template>

<script>
import Nav from './components/Nav.vue'
export default {
  name: 'Home',
  components: {
    Nav
  }
}
</script>

<style lang="scss">
$primar: '#5f2882';
@import './node_modules/bootstrap/scss/bootstrap';
</style>
