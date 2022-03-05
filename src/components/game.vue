<script setup>
import { ref, onMounted, reactive } from 'vue'
import headerComponent from'./header.vue'
import gameTable from'./gameTable.vue'

const props = defineProps({
    isMain:Boolean
})

const today = Math.floor((Date.now()-(new Date()).getTimezoneOffset()*60000)/86400000)

const wordsArray = reactive(localStorage.getItem('wordsArray') && props.isMain ? localStorage.getItem('wordsArray').split(',') : [""]),
      playedCount = ref(parseInt(localStorage.getItem('playedCount') || "0")),
      winCount = ref(parseInt(localStorage.getItem('winCount') || 0)),
      winRate = ref(parseInt(localStorage.getItem('winRate') || 0)),
      currentStreak = ref(parseInt(localStorage.getItem('currentStreak') || 0)),
      maxStreak = ref(parseInt(localStorage.getItem('maxStreak') || 0)),
      lastClearedDate = ref(parseInt(localStorage.getItem('lastClearedDate') || 0)),
      lastPlayedDate = ref(parseInt(localStorage.getItem('lastPlayedDate') || 0)),
      clearState = ref(props.isMain ? parseInt(localStorage.getItem('clearState')) : 0),
      scoreText = ref(""),
      resultText = ref(""),
      answerWord = ref("")


const clearAction = (st, scTxt, rsTxt, ansW) => {
    clearState.value = st
    console.log(clearState.value)
    scoreText.value = scTxt
    resultText.value = rsTxt
    answerWord.value = ansW
    if(lastPlayedDate.value != today){
        lastPlayedDate.value = today
        playedCount.value++
        if(clearState.value === 1){
            winCount.value++
            currentStreak.value++
            lastClearedDate.value = today
        }

        winRate.value = Math.floor(winCount.value/playedCount.value * 100)
        maxStreak.value = Math.max(maxStreak.value, currentStreak.value)
        if(!props.isMain)return // メインゲームでなければ更新しない
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
    if(lastClearedDate.value !== today-1)currentStreak.value = 0;
})
        // 0:Unfinished , 1:Clear, 2:Over
</script>


<template>
<headerComponent :answerWord="answerWord" :clearState="clearState" :scoreText="scoreText" :resultText="resultText" :playedCount="playedCount" :winRate="winRate" :currentStreak="currentStreak" :maxStreak="maxStreak"></headerComponent>
<gameTable :isMain="isMain" :pWordsArray="wordsArray" :clearState="clearState" @updateClearState="clearAction"></gameTable>
</template>


<style scoped>
</style>
