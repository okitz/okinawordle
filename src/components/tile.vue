<script setup>
import { ref, computed, watch} from 'vue'
import 'animate.css';

const props = defineProps({
  letter: String,
  state: Number, // [idle, absent, present, correct]
  index: Number
})

const flipped = computed(() => props.state > 0)

const frontTileStyle = computed(() => {
  const ret = (!props.letter || props.letter.length === 0) ? {
    outline: `0.1rem solid grey`,
    outlineOffset: `-0.1rem`
   } : {
    color:`black`,
    outline: `0.1rem solid black`,
    outlineOffset: `-0.1rem`,
    animation: `pulse`,
    animationDuration: `300ms`
   }
   if(!flipped.value)ret.transform = `rotateX(${0}deg)`
   else{
      ret.transition = `transform 400ms linear ${260*props.index}ms`
      ret.transform = `rotateX(${180}deg)`
   }
  return ret
})

const backTileClass = computed(() => {
  const ret = {};
  ret.backgroundColor = ["white", "grey", "#c9b458", "#6aaa64"][props.state]
  ret.color = `white`
  if(!flipped.value)ret.transform = `rotateX(${180}deg)`
  else{
    ret.transition = `transform 400ms linear ${260*props.index}ms`
    ret.transform = `rotateX(${0}deg)`
  }
  return ret;
})

</script>

<template>
<div class="tileWrapper">
  <div :style="frontTileStyle">{{letter}}</div>
  <div :style="backTileClass">{{letter}}</div>
</div>
</template>

<style scoped>
.tileWrapper{
  position:relative;
  margin:0.1rem;
  text-align: center;
  font-size: 1rem;
  width:2rem;
  height:2rem;
  line-height:2rem;
}

.tileWrapper div{
  position: absolute;
  backface-visibility:hidden;
  width:100%;
  height:100%;
}

</style>
