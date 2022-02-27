<script setup>
import { ref, computed, watch} from 'vue'
import 'animate.css';

const props = defineProps({
  letter: String,
  state: Number // [idle, absent, present, correct]
})

const flipped = computed(() => props.state >= 0)

const frontTileStyle = computed(() => {
  const ret = (props.letter.length === 0) ? {
    outline: `0.2rem solid grey`,
    outlineOffset: `-0.2rem`
   } : {
    outline: `0.2rem solid black`,
    outlineOffset: `-0.2rem`,
    animation: `pulse`,
    animationDuration: `300ms`
   }
   if(!flipped)ret.transform = `rotateX(${0}deg)`
   else{
      ret.animation = `animation: flip 0.5s;`
   }
  return ret
})

const backTileClass = computed(() => {
  const ret = {};
  ret.backgroundColor = ["white", "black", "yellow", "green"][props.state]
   if(!flipped)ret.transform = `rotateX(${180}deg)`
   else{
      ret.animation = `animation: flip 0.5s;`
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

@keyframes flip {
  0%   { transform: rotateX(180deg); }
  100% { transform: rotate(0deg); }
}




</style>
