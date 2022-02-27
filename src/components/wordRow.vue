<script setup>
import { ref } from 'vue'
import tile from './tile.vue'

defineProps({
  wordData: Array
})

</script>

<template>
<div class="tilesWrapper">
<tile v-for="(letterData,index) in wordData" :letter="letterData.letter" :state="letterData.state" :index="index" :key="index"></tile>
</div>
</template>

<style scoped>
.tilesWrapper{
  display: flex;
  flex-wrap: wrap;
}

</style>
