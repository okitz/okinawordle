<script setup>
import { ref, computed } from 'vue'
import 'animate.css';

const props = defineProps({
  letter: String,
  state: Number // [empty, absent, present, correct]
})

const tileClass = computed(() => {
  console.log(props)

  if(props.state > 0)return "letterTile"
  else return "emptyTile"
})

</script>

<template>
<div class="tile animate__animated animate__bounce" :class="tileClass">
    <transition name="bounce">
    <p v-if="letter">{{letter}}</p>
  </transition>
</div>
</template>

<style scoped>
.tile{
  margin:0.1vw;
  text-align: center;
  font-size: 0.01vm;
}
.letterTile{
  width:3.8vw;
  height:3.8vw;
  background-color: green;
}

.emptyTile{
  width:3vw;
  height:3vw;
  border: 0.4vw solid grey;
}
</style>
