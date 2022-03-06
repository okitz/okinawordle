<script setup>
// This starter template is using Vue 3 <script setup> SFCs
// Check out https://v3.vuejs.org/api/sfc-script-setup.html#sfc-script-setup
import game from './components/game.vue'
import {ref} from 'vue'
const mode = ref(true)

const switchMode = () => {
  mode.value = !mode.value
}
</script>

<template>

<game :isMain="true" v-if="mode" @switchMode="switchMode"></game>
<game :isMain="false" v-if="!mode" @switchMode="switchMode"></game>
</template>

<style>
#app {
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
</style>
