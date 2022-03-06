<script setup>
import { ref, onMounted, reactive } from 'vue'
import headerComponent from'./header.vue'
import gameTable from'./gameTable.vue'

const props = defineProps({
    isMain:Boolean
})

const emit = defineEmits(['switchMode'])
const today = Math.floor((Date.now()-(new Date()).getTimezoneOffset()*60000)/86400000)

const playedCount = ref(parseInt(localStorage.getItem('playedCount') || "0")),
      winCount = ref(parseInt(localStorage.getItem('winCount') || 0)),
      winRate = ref(parseInt(localStorage.getItem('winRate') || 0)),
      currentStreak = ref(parseInt(localStorage.getItem('currentStreak') || 0)),
      maxStreak = ref(parseInt(localStorage.getItem('maxStreak') || 0)),
      lastClearedDate = ref(parseInt(localStorage.getItem('lastClearedDate') || 0)),
      lastPlayedDate = ref(parseInt(localStorage.getItem('lastPlayedDate') || 0)),
      lastSavedDate = ref(parseInt(localStorage.getItem('lastSavedDate') || 0)),
      clearState = ref(props.isMain ? parseInt(localStorage.getItem('clearState')) : 0),
      wordsArray = ref(localStorage.getItem('wordsArray') && props.isMain && lastSavedDate.value === today ? localStorage.getItem('wordsArray').split(',') : [""]),
      scoreText = ref(""),
      resultText = ref(""),
      answerWord = ref("")


const clearAction = (st, scTxt, rsTxt, ansW) => {
    clearState.value = st
    scoreText.value = scTxt
    resultText.value = rsTxt
    answerWord.value = ansW

    if(props.isMain && lastPlayedDate.value != today){
        lastPlayedDate.value = today
        playedCount.value++
        if(clearState.value === 1){
            winCount.value++
            currentStreak.value++
            lastClearedDate.value = today
        }

        winRate.value = Math.floor(winCount.value/playedCount.value * 100)
        maxStreak.value = Math.max(maxStreak.value, currentStreak.value)
        localStorage.setItem('playedCount', playedCount.value)
        localStorage.setItem('winCount', winCount.value)
        localStorage.setItem('winRate', winRate.value)
        localStorage.setItem('currentStreak',currentStreak.value)
        localStorage.setItem('maxStreak',maxStreak.value)
        localStorage.setItem('lastPlayedDate', lastPlayedDate.value)
        localStorage.setItem('lastClearedDate', lastClearedDate.value)
        localStorage.setItem('clearState', clearState.value)
    }
}

onMounted(() => {
    if(lastClearedDate.value < today-1)currentStreak.value = 0;
    if(lastSavedDate.value !== today){
        localStorage.setItem("wordsArray", "")
        lastSavedDate.value = today
        clearState.value = 0
        localStorage.setItem('lastSavedDate', lastSavedDate.value)
        localStorage.setItem('clearState', clearState.value)
    }    
})

const switchMode = () =>{
    emit('switchMode')
}

</script>


<template>
<headerComponent
@switchMode="switchMode"
:isMain="isMain" :answerWord="answerWord" :clearState="clearState" :scoreText="scoreText" :resultText="resultText" :playedCount="playedCount" :winRate="winRate" :currentStreak="currentStreak" :maxStreak="maxStreak"
></headerComponent>
<gameTable :isMain="isMain" :pWordsArray="wordsArray" :clearState="clearState" @updateClearState="clearAction"></gameTable>
</template>


<style scoped>
</style>
