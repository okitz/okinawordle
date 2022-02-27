<script setup>
import { ref } from 'vue'

defineProps({
  letter: String
})

</script>

<template>
<button>{{letter}}</button>
</template>

<style scoped>

</style>
