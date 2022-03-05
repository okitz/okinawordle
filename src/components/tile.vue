<script setup>
import { ref, computed, watch} from 'vue'
import 'animate.css';

const props = defineProps({
  letter: String,
  state: Number, // [idle, absent, present, correct]
  index: Number
})
const emit = defineEmits(['animation-end'])

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
  ret.backgroundColor = ["#FFFFFF", "#787C7E", "#C9B458", "#6AAA64"][props.state]
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
  <div :style="backTileClass" @transitionend ="emit('animation-end')">{{letter}}</div>
</div>
</template>

<style scoped>
.tileWrapper{
  position:relative;
  margin:0.11rem;
  text-align: center;
  font-size: 1.5rem;
  width:2.0rem;
  height:2.0rem;
  line-height:2rem;

  user-select: none;
}

.tileWrapper div{
  position: absolute;
  backface-visibility:hidden;
  width:100%;
  height:100%;
  font-family: 'Kiwi Maru';

}

</style>
