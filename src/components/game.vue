<script setup>
import { ref, computed, reactive } from 'vue'
import wordRow from './wordRow.vue'
import keyButton from './keyButton.vue'

defineProps({
})

const RAW = 6, LENGTH = 5,
      letters = ref( ['ア', 'イ', 'ウ', 'エ', 'オ', 'カ', 'キ', 'ク', 'ケ', 'コ', 'サ', 'シ', 'ス', 'セ', 'ソ', 'タ', 'チ', 'ツ', 'テ', 'ト', 'ナ', 'ニ', 'ヌ', 'ネ', 'ノ', 'ハ', 'ヒ', 'フ', 'ヘ', 'ホ', 'マ', 'ミ', 'ム', 'メ', 'モ', 'ヤ', '','ユ','', 'ヨ', 'ラ', 'リ', 'ル', 'レ', 'ロ', 'ワ','ヲ', 'ン', '','ー','ガ', 'ギ', 'グ', 'ゲ', 'ゴ', 'ザ', 'ジ', 'ズ', 'ゼ', 'ゾ', 'ダ', 'ヂ', 'ヅ', 'デ', 'ド', 'バ', 'ビ', 'ブ', 'ベ', 'ボ', 'パ', 'ピ', 'プ', 'ペ', 'ポ', 'ァ', 'ィ', 'ゥ', 'ェ', 'ォ',  'ャ', 'ュ', 'ョ','ッ',''])
      
const letterState = ref([])
for(let i=0;i<letters.value.length;i++)letterState.value.push(0);

const wordsArray = reactive([""])
const answer = "アキステノ"
const wordsData = computed(() => {
    const returnArray = [];
    for(let i=0;i<RAW;i++){
        const a = []
        for(let j=0;j<LENGTH;j++)a.push({letter:'', state:0})
        returnArray.push(a);
    }

    wordsArray.forEach((word, wordIndex) => {
        if(wordIndex >= RAW)return;
        for(let i = 0;i < word.length;i++){
            const c = word[i]
            let cnt = 0;
            for(let j = 0;j < word.length;j++){
                if(answer[j] === c && answer[j] !== word[j])cnt++
                if(j < i && word[j] === c)cnt--;
            }

            let state;
            if(wordIndex === wordsArray.length - 1)state = 0
            else if(answer[i] === c)state = 3
            else if(cnt > 0)state = 2
            else state = 1;
            returnArray[wordIndex][i] = {
                letter:c,
                state:state
            };
        }
    })
    return returnArray;
})


const updateUsedLetters = () => {
    const ret = []
    for(let i=0;i<letters.value.length;i++)ret.push(0);
    for(let wordArray of wordsData.value){
        for(let letterData of wordArray){
            if(letterData.letter !== '')ret[letters.value.indexOf(letterData.letter)] = Math.max(letterData.state,ret[letters.value.indexOf(letterData.letter)])
        }
    }
    return ret;
}

const usedLetters = ref(updateUsedLetters())



const addLetterToRaw = (letter) => {
    const lastInd = wordsArray.length - 1;
    wordsArray[lastInd] = (wordsArray[lastInd] + letter).slice(0,LENGTH)
}

const confirmButton = () => {
    const lastInd = wordsArray.length - 1;
    if(wordsArray[lastInd].length === LENGTH)wordsArray.push('')
}

const deleteButton = () => {
    const lastInd = wordsArray.length - 1, len = wordsArray[lastInd].length;
    if(len > 0)wordsArray[lastInd] = wordsArray[lastInd].slice(0,len-1)

}
</script>


<template>
<word-row v-for="(wordData,index) in wordsData" :wordData="wordData" :key="index" v-on:animation-end="usedLetters = updateUsedLetters()"></word-row>

<template v-for="i in 5">
<div class="keyButtonsWrapper">
<template  v-for="(letter,index) in letters" :key="index">
<keyButton v-if="index%5 === i-1" :letter="letter" :state="usedLetters[index]"  @click="addLetterToRaw(letter)"></keyButton>
</template>
</div>
</template>

<div class="keyButtonsWrapper">
<keyButton :letter="'Enter'" :state="4" @click="confirmButton">confirm</keyButton>
<keyButton :letter="'Delete'" :state="4" @click="deleteButton">delete</keyButton>
</div>
</template>

<style scoped>
.keyButtonsWrapper{
  display: flex;
  flex-wrap: wrap;
}
</style>
