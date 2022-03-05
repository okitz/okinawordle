<script setup>
import { ref, computed } from 'vue'

const props = defineProps({
  letter: String,
  state: Number
})

const styleByState = computed(() => {
  return {
    width: props.state <= 3 ? '1.6rem': '5rem',
    backgroundColor: ["#D3D6DA", "#787C7E", "#C9B458", "#6AAA64","#D3D6DA"][props.state],
    color: props.state === 0 || props.state === 4 ? "black" : "white"
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
