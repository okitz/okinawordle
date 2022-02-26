<script setup>
import { ref } from 'vue'
import wordRow from './wordRow.vue'

defineProps({
})

const words = [[{letter:'あ', state:1},{letter:'い', state:2}, {letter:'う', state:3},{letter:'', state:0}]]

</script>

<template>
<word-row v-for="(wordData,index) in words" :wordData="wordData" :key="index"></word-row>
</template>

<style scoped>
</style>
