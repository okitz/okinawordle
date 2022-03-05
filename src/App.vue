<script setup>
// This starter template is using Vue 3 <script setup> SFCs
// Check out https://v3.vuejs.org/api/sfc-script-setup.html#sfc-script-setup
import game from './components/game.vue'

</script>

<template>
<game :isMain="true"></game>
<!-- <game :isMain="false"></game> -->
<!-- <game :isMain="false"></game> -->
</template>

<style>
#app {
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
</style>
