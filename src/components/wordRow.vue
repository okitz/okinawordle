<script setup>
import { ref } from 'vue'
import tile from './tile.vue'

defineProps({
  wordData: Array
})


</script>

<template>
<div class="tilesWrapper">
<tile v-for="(charData,index) in wordData" :letter="charData.letter" :state="charData.state" :key="index"></tile>
</div>
</template>

<style scoped>
.tilesWrapper{
  display: flex;
  flex-wrap: wrap;

}

</style>
