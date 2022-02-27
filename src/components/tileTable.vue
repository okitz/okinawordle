<script setup>
import { ref, computed, reactive } from 'vue'
import wordRow from './wordRow.vue'
import letterKey from './letterKey.vue'

defineProps({
})

const RAW = 6, LENGTH = 5,
      letters = ref(['あ', 'い', 'う', 'え', 'お', 'か', 'き', 'く', 'け', 'こ', 'さ', 'し', 'す', 'せ', 'そ', 'た', 'ち', 'つ', 'て', 'と', 'な', 'に', 'ぬ', 'ね', 'の', 'は', 'ひ', 'ふ', 'へ', 'ほ', 'ま', 'み', 'む', 'め', 'も', 'や', 'ゆ', 'よ', 'ら', 'り', 'る', 'れ', 'ろ', 'わ', 'ゐ', 'ゑ', 'を', 'ん', 'が', 'ぎ', 'ぐ', 'げ', 'ご', 'ざ', 'じ', 'ず', 'ぜ', 'ぞ', 'だ', 'ぢ', 'づ', 'で', 'ど', 'ば', 'び', 'ぶ', 'べ', 'ぼ', 'ぱ', 'ぴ', 'ぷ', 'ぺ', 'ぽ', 'ぁ', 'ぃ', 'ぅ', 'ぇ', 'ぉ', 'っ', 'ゃ', 'ゅ', 'ょ'])

const wordsArray = reactive([""])
const answer = "うんちくま"
const wordsData = computed(() => {
    const returnArray = [];
    for(let i=0;i<RAW;i++){
        const a = []
        for(let j=0;j<LENGTH;j++)a.push({letter:'', state:0})
        returnArray.push(a);
    }

    wordsArray.forEach((word, wordIndex) => {
        if(wordIndex >= RAW)return;
        for(let index = 0;index < word.length;index++){
            const c = word[index];
            let state;
            if(wordIndex === wordsArray.length - 1)state = 0
            else if(answer[index] === c)state = 3
            else if(answer.indexOf(c) >= 0)state = 2
            else state = 1;
            returnArray[wordIndex][index] = {
                letter:c,
                state:state
            };
        }
    })
    return returnArray;
})

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
<word-row v-for="(wordData,index) in wordsData" :wordData="wordData" :key="index"></word-row>
<button type="button" @click="confirmButton">confirm</button>
<button type="button" @click="deleteButton">delete</button>
<letterKey v-for="(letter,index) in letters" :letter="letter" :key="index" @click="addLetterToRaw(letter)">{{letter}}</letterKey>
</template>

<style scoped>
</style>
