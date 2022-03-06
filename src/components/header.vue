<script setup>
import { ref, computed, watch } from 'vue'
import { QuestionFilled,Setting,DataAnalysis, Switch } from '@element-plus/icons-vue'

const props = defineProps({
  isMain:Boolean,
  answerWord:String,
  clearState:Number,
  scoreText:String,
  resultText:String,
  playedCount:Number,
  winRate:Number,
  currentStreak:Number,
  maxStreak:Number
})

const emit = defineEmits(['switchMode'])

const guideDialogVisible = ref(false),
      settingDialogVisible = ref(false),
      statsDialogVisible = ref(false)  

watch(
  () => props.clearState,
  () => {
    if(props.clearState)statsDialogVisible.value = true
  }
)

const shareText = computed(() => {
  return `OKINAWordle ${props.scoreText}\n\n${props.resultText}\n${location.href}\n`
})
const shareOnTwitter = () => {
  const url = `https://twitter.com/intent/tweet?text=${shareText.value}&hashtags=OKINAWordle,沖縄Wordle`
  window.open(encodeURI(url))
}
const copyToClipboard = () => {
  navigator.clipboard.writeText(shareText.value)
}
const switchMode = () => {
  emit('switchMode')
}
</script>
<template>
<el-header id="headerWrapper">
  <div>
  <div class="mainTitle">OKINAWordle</div>
  <div v-if="isMain" class="modeTitle">日替わりモード</div>
  <div v-if="!isMain" class="modeTitle">ランダムモード</div>
  </div>
  <div class="buttons">
    <el-button type="default" :icon="QuestionFilled" circle plain @click="guideDialogVisible = true"></el-button>
    <el-button type="default" :icon="Setting" circle plain @click="settingDialogVisible = true"></el-button>
    <el-button type="default" :icon="DataAnalysis" circle plain @click="statsDialogVisible = true;"></el-button>
    <el-button type="default" :icon="Switch" round plain @click="switchMode"><span style="font-family: 'Fredoka','Kiwi Maru', sans-serif,serif;">モード切替</span></el-button>
  </div>
</el-header>
  
  <div class="dialogsWrapper">
  <el-dialog v-model="guideDialogVisible" title="HOW TO PLAY" :width="'min(100%,40rem)'">
    <div class="dialogContents">
    <div>Wordleクローンの沖縄語版です</div>
    <div>
    <div style="color:#5F995A;font-weight: bold;font-size: 1.5rem;">ルール</div>
    <ul>
    <li>お題は日替わりでカナ4文字の名詞です</li>
    <li>単語として登録されていない4文字も入力できます</li>
    <li>辞書に登録されている単語が入力されている場合、ⓘアイコンから情報を見ることができます</li>
    <li>ランダムモードのお題・入力履歴はモードを切り替えるたびにリセットされます</li>
    </ul>
    </div>

    <div>
    <div style="color:#B4A24F;font-size: 1.5rem;">注意事項</div>
    <ul>
    <li>登録辞書には沖縄語の中でも首里地方で使用されている/いた語が収録されています</li>
    <li>方言辞書データをほぼ校閲無しで利用しているため、一部不適切な表現が含まれる可能性があります</li>
    <li>語のカナ表記については下記資料に掲載されている対応表を参考にしていますが、必ずしも本来の沖縄語における発音の正確な転写ではありません</li>
    <li>不具合等の報告は<a href="https://twitter.com/cacturne_you_">制作者Twitter</a>までお願いします</li>
    </ul>
    </div>

    <div>当サイトはJosh Wardle氏開発の<a href="https://www.nytimes.com/games/wordle/index.html">Wordle</a>にインスパイアされています。また、UI設計等において<a href="https://twitter.com/giga_yadoran">やど氏</a>制作の<a href="https://wordle.mega-yadoran.jp/">ポケモンWordle</a>を参考にさせていただきました。
    </div>
    <div>語彙データは<a href="https://mmsrv.ninjal.ac.jp/okinawago/">沖縄語辞典 データ集</a>を加工の上で利用しています。本データ集は<a href="https://repository.ninjal.ac.jp/?action=pages_view_main&active_action=repository_view_main_item_detail&item_id=2282&item_no=1&page_id=13&block_id=21">©国立国語研究所 資料集5『沖縄語辞典』第9刷（1963年刊行，2001年改訂）</a>に基づき作成され、<a href="https://creativecommons.org/licenses/by/4.0/deed.ja">クリエイティブ・コモンズ 表示 4.0 国際 ライセンス</a>の下に提供されています。
    </div>
    <div style="text-align:right">Ver.1.0.0</div>
    </div>
  </el-dialog>

  
  <el-dialog v-model="settingDialogVisible" title="SETTING"  :width="'min(100%,40rem)'">
  <div class="dialogContents">未実装です</div>
  </el-dialog>

  <el-dialog v-model="statsDialogVisible" title="STATISTICS (DAILY)"  :width="'min(100%,40rem)'">
  <div class="statsWrapper">
    <div><div class="scores">{{playedCount}}</div>Played</div>
    <div><div class="scores">{{winRate}}</div>Win %</div>
    <div><div class="scores">{{currentStreak}}</div>Current<br>Streak</div>
    <div><div class="scores">{{maxStreak}}</div>Max<br>Streak</div>

    <div class="statsButton shareButton" @click="shareOnTwitter">
      <!-- <img src="../assets/Logo blue.svg" width="50" height="50"> -->
      TWEET
    </div>
    <div class="statsButton copyButton" @click="copyToClipboard">
      COPY
    </div>
    <div v-if="clearState === 1" class="clearDisplay">
      CLEAR!
    </div>
    <div v-if="clearState === 2" class="clearDisplay">
      {{answerWord}}
    </div>
  </div>

  </el-dialog>
  </div>
</template>

<style scoped>
#headerWrapper{
  display:grid;
  height:6rem;
  grid-template-columns:15rem 0.05fr 0.95fr;
  grid-template-rows:0.5fr 0.5fr;
  border-bottom: 0.3rem solid var(--el-border-color-base);

  font-family: 'Fredoka','Kiwi Maru', sans-serif,serif;

}
.statsWrapper{
  margin-left: 5rem;
  margin-right: 5rem;
  display:grid;
  grid-template-columns:0.25fr 0.25fr 0.25fr 0.25fr;
  grid-template-rows: 1fr 5rem;

  font-size: 1rem;
}
.scores{
  font-size:2rem;
}

.statsButton{
  border: 0.5rem solid;
  border-radius: 8%;
  margin-top:1rem;
  margin-left:1.5rem;
  margin-right:1.5rem;
  font-size:min(2rem, 3.5vw);
  
  display: grid;
  /* grid-template-columns:0.3fr 0.7fr; */
  align-items: center;
}
.statsButton:hover {
  background-color: grey;
}
.shareButton{
  grid-column:1/3;
  background-color: white;
  border-color: #1DA1F2;
}
.copyButton{
  grid-column:3/5;
  background-color: white;
  border-color: #f2cf1d
;
}
.clearDisplay{
  grid-column:1/5;
  margin: 1rem;
  font-size: 3.5rem;
  color: #6AAA64;
  font-family: 'Fredoka','Kiwi Maru', sans-serif,serif;
}
.dialogsWrapper{
  font-family: 'Fredoka',sans-serif;
}
.mainTitle{
  font-size:2.5rem;
  justify-self:safe right;
  grid-column: 1 / 2;
}
.modeTitle{
  font-size:1rem;
  align-self: start;
  grid-column: 1 / 2;
}
.buttons{
  grid-column: 3 / 4;
  justify-self:left;
}
button > span{
  font-family: 'Fredoka','Kiwi Maru', sans-serif,serif;

}
.dialogContents{
  text-align:left;
  font-family: 'Kiwi Maru',serif;
}
.dialogContents > div{
  padding:1rem;
  border-bottom: 0.2rem dashed var(--el-border-color-base);
}
ul{
  margin-top:0.6rem;
}

</style>
