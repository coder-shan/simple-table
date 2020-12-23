<template>
  <div id="app">
    <HellWord v-if="isRouterAlive"></HellWord>
  </div>
</template>

<script>
  import HellWord from './components/HelloWorld.vue'
  export default {
    name: 'app',
    components: {
      HellWord
    },
    provide () {
      return {
        reload: this.reload
      }
    },
    data() {
      return {
        isRouterAlive: true
      };
    },
    methods: {
      reload() {
        this.isRouterAlive = false
        this.$nextTick(function() {
          this.isRouterAlive = true
        })
      }
    },
  }
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
</style>
