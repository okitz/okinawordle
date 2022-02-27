<script setup>
import { ref } from 'vue'
import wordRow from './wordRow.vue'

defineProps({
})

const words = ref([[{letter:'あ', state:0},{letter:'い', state:2}, {letter:'う', state:3},{letter:'', state:0}]])

setTimeout(()=>{words.value[0][0].state = '1';console.log(words.value[0][0])}, 5000) 
</script>

<template>
<word-row v-for="(wordData,index) in words" :wordData="wordData" :key="index"></word-row>
</template>

<style scoped>
</style>
