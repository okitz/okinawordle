<script setup>
import { ref, computed, reactive } from 'vue'
import wordRow from './wordRow.vue'
import keyButton from './keyButton.vue'
import dictJSON from '../assets/wordDict.json'
import listJSON from '../assets/wordList.json'

const ROW = 16, LENGTH = 5,
      letters = ref( ['ア', 'イ', 'ウ', 'エ', 'オ', 'カ', 'キ', 'ク', 'ケ', 'コ', 'サ', 'シ', 'ス', 'セ', 'ソ', 'タ', 'チ', 'ツ', 'テ', 'ト', 'ナ', 'ニ', 'ヌ', 'ネ', 'ノ', 'ハ', 'ヒ', 'フ', 'ヘ', 'ホ', 'マ', 'ミ', 'ム', 'メ', 'モ', 'ヤ', '','ユ','', 'ヨ', 'ラ', 'リ', 'ル', 'レ', 'ロ', 'ワ','ヲ', 'ン', '','ー','ガ', 'ギ', 'グ', 'ゲ', 'ゴ', 'ザ', 'ジ', 'ズ', 'ゼ', 'ゾ', 'ダ', 'ヂ', 'ヅ', 'デ', 'ド', 'バ', 'ビ', 'ブ', 'ベ', 'ボ', 'パ', 'ピ', 'プ', 'ペ', 'ポ', 'ァ', 'ィ', 'ゥ', 'ェ', 'ォ',  'ャ', 'ュ', 'ョ','ッ','ヮ','','','','','','','','','','','','','','',''])
      
const letterState = ref([])
for(let i=0;i<letters.value.length;i++)letterState.value.push(0);

const wordsArray = reactive([""])
const answerList = listJSON.map((word, ID) => {return {'word':word, 'ID':ID}}).filter((wordObj) => wordObj.word.length === LENGTH)
const answerNumber = Math.floor(Math.random()*answerList.length)
const answerWord = answerList[answerNumber].word
const answerID = ref(answerList[answerNumber].ID)

console.log(answerWord)
const wordsData = computed(() => {
    const returnArray = [];
    for(let i=0;i<ROW;i++){
        const a = []
        for(let j=0;j<LENGTH;j++)a.push({letter:'', state:0})
        returnArray.push(a);
    }

    wordsArray.forEach((word, wordIndex) => {
        if(wordIndex >= ROW)return;
        for(let i = 0;i < word.length;i++){
            const c = word[i]
            let cnt = 0;
            for(let j = 0;j < word.length;j++){
                if(answerWord[j] === c && answerWord[j] !== word[j])cnt++
                if(j < i && word[j] === c)cnt--;
            }

            let state;
            if(wordIndex === wordsArray.length - 1)state = 0
            else if(answerWord[i] === c)state = 3
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
            if(letterData.letter !== ''){
                console.log(letters.value.indexOf(letterData.letter), ret.length, ret[letters.value.indexOf(letterData.letter)],letterData.state)
                ret[letters.value.indexOf(letterData.letter)] = Math.max(letterData.state,ret[letters.value.indexOf(letterData.letter)])
            
                console.log(letters.value.indexOf(letterData.letter), ret.length, ret[letters.value.indexOf(letterData.letter)],letterData.state)
            }
        }
    }
    return ret;
}

const usedLetters = ref(updateUsedLetters())



const addLetterToRow = (letter) => {
    const lastInd = wordsArray.length - 1;
    wordsArray[lastInd] = (wordsArray[lastInd] + letter).slice(0,LENGTH)
}

const confirmButton = () => {
    console.log(343)
    const lastInd = wordsArray.length - 1;
    if(wordsArray[lastInd].length === LENGTH){
        wordsArray.push('')
    }
}

const deleteButton = () => {
    const lastInd = wordsArray.length - 1, len = wordsArray[lastInd].length;
    if(len > 0)wordsArray[lastInd] = wordsArray[lastInd].slice(0,len-1)

}

const getMeanings = (wordData) => {
    const word = dictJSON.find((x) => x.title === wordData.map(x => x.letter).join(''),)
    return word ? word.meanings : null
}

const hintTexts = computed(() => {
    return []
})
</script>


<template>

<div class="tableWrapper">
<div id="tableLeftColumn">
    <template v-for="(wordData,index) in wordsData" :key="index">
    <word-row  v-if="index < ROW/2" :wordData="wordData" :meanings="getMeanings(wordData)" v-on:animation-end="usedLetters = updateUsedLetters()"></word-row>
    </template>
</div>
<div id="tableRightColumn">
    <template v-for="(wordData,index) in wordsData" :key="index">
    <word-row  v-if="index >= ROW/2" :wordData="wordData" :meanings="getMeanings(wordData)" v-on:animation-end="usedLetters = updateUsedLetters()"></word-row>
    </template>
</div>
</div>

<div class="keyBoardWrapper">
<div class="keyButtonsWrapper">
<keyButton :letter="'ENTER'" :state="4" @click="confirmButton">confirm</keyButton>
<keyButton :letter="'DELETE'" :state="4" @click="deleteButton">delete</keyButton>
    <el-popover placement="bottom" trigger="click" width="50vw">
      <template #reference>
        <keyButton :letter="'ヒント'" :state="4"></keyButton>
      </template>
      <div>
          答えの単語の意味
          <div v-for="meaning in dictJSON[answerID].meanings">・{{meaning}}</div>
      </div>
      <div v-for="(hintText,index) in hintTexts" :key="index">
      ・{{hintText}}
      </div>
    </el-popover>
</div>


<template v-for="i in 5">
<div class="keyButtonsWrapper">
<template  v-for="(letter,index) in letters.slice(0,50)" :key="index">
<keyButton v-if="index%5 === i-1" :letter="letter" :state="letter === '' ? -1 : usedLetters[index]"  @click="addLetterToRow(letter)"></keyButton>
</template>
</div>
</template>
<template v-for="i in 5">
<div class="keyButtonsWrapper">
<template  v-for="(letter,index) in letters.slice(50)" :key="index">
<keyButton v-if="index%5 === i-1" :letter="letter" :state="letter === '' ? -1 : usedLetters[50+index]"  @click="addLetterToRow(letter)"></keyButton>
</template>
</div>
</template>

</div>

</template>


<style scoped>

.keyBoardWrapper{
  display: grid;
  /* grid-template-columns:0.49fr 0.02fr 0.49fr; */
  /* grid-template-rows:0.49fr 0.02fr 0.49fr; */
}
.keyButtonsWrapper {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
}
.tableWrapper{
    display: grid;
    grid-template-columns:0.49fr 0.02fr 0.49fr
}
#tableLeftColumn{
    grid-column: 1/2;
    justify-self:right;
}
#tableRightColumn{
    grid-column: 3/4;
    justify-self:left;
}
</style>
