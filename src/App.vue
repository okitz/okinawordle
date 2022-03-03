<script setup>
// This starter template is using Vue 3 <script setup> SFCs
// Check out https://v3.vuejs.org/api/sfc-script-setup.html#sfc-script-setup
import game from './components/game.vue'
import headerComponent from'./components/header.vue'

</script>

<template>
<el-header id="header">
<headerComponent></headerComponent>
</el-header>
<el-main>
  <game></game>
</el-main>
  
</template>

<style>
#app {
  font-family: Helvetica, Arial, 'Kiwi Maru', serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

#header{
  border-bottom: 0.3vw solid var(--el-border-color-base);
}
</style>
