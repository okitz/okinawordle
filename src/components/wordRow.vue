<script setup>
import { ref } from 'vue'
import tile from './tile.vue'

defineProps({
  wordData: Array,
  length:Number
})


</script>

<template>
<div class="tilesWrapper">
<tile v-for="(letterData,index) in wordData" :letter="letterData.letter" :state="letterData.state" :key="index"></tile>
</div>
</template>

<style scoped>
.tilesWrapper{
  display: flex;
  flex-wrap: wrap;

}

</style>
