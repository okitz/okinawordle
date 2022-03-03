<script setup>
import { ref } from 'vue'
import tile from './tile.vue'

defineProps({
  wordData: Array
})
const emit = defineEmits(['animation-end'])
const animationCatched = ref(0)

</script>

<template>
<div class="tilesWrapper">
<tile v-for="(letterData,index) in wordData" :letter="letterData.letter" :state="letterData.state" :index="index" :key="index" @animation-end="++animationCatched === 5 ? emit('animation-end') : null"></tile>
</div>
</template>

<style scoped>
.tilesWrapper{
  display: flex;
  flex-wrap: wrap;
}

</style>
