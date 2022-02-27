<script setup>
import { ref, computed } from 'vue'
import 'animate.css';

const props = defineProps({
  letter: String,
  state: Number, // [idle, absent, present, correct]
})

const frontTileClass = computed(() => {
  if(props.letter.length === 0)return "emptyTile"
  else return "idleTile"
})

const backTileClass = computed(() => {
  return [null, "absentTile", "presentTile", "correctTile"][props.state]
})

const unflipped = computed(() => {
  return  props.state === 0;
})
</script>

<template>
<div class="tileWrapper">
  <div :style="{transform:`rotateX(${unflipped ? 0 : 180}deg)`}" :class="frontTileClass">{{letter}}</div>
  <div :style="{transform:`rotateX(${unflipped ? 180 : 0}deg)`}" :class="backTileClass">{{letter}}</div>
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

}
.tileWrapper div{
  position: absolute;
  backface-visibility:hidden;
  width:100%;
  height:100%;
}


.emptyTile{
  border: 0.2rem solid grey;
  box-sizing: border-box;
}
.idleTile{
  border: 0.2rem solid black;
  box-sizing: border-box;
  animation: pulse; 
  animation-duration: 200ms;
}
.absentTile{
  background-color: grey;
}
.presentTile{
  background-color: yellow;
}
.correctTile{
  background-color: green;
}



</style>
