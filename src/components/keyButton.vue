<script setup>
import { ref, computed } from 'vue'

const props = defineProps({
  letter: String,
  color: Number,
  isLong: Boolean
})

const styleByState = computed(() => {
  return {
    width: props.isLong ? '5rem': '1.6rem',
    backgroundColor: ["#D3D6DA", "#787C7E", "#C9B458", "#6AAA64"][props.color],
    color: props.color === 0 || props.color === 3 ? "black" : "white"
  }
})
</script>

<template>
  <div class="keyButton" :style="styleByState">{{letter}}</div>
</template>

<style scoped>
.keyButton{
  height:2.2rem;
  margin:0.1rem;
  text-align: center;
  font-size: 1.1rem;
  line-height:2rem;
  border-radius: 0.3rem;
  
  font-family: 'Fredoka','Kiwi Maru',sans-serif;
  user-select: none;

}
</style>
