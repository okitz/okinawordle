<script setup>
import { ref, computed } from 'vue'
import { QuestionFilled,Setting,DataAnalysis } from '@element-plus/icons-vue'

const props = defineProps({
})

const guideDialogVisible = ref(false),
      settingDialogVisible = ref(false),
      statsDialogVisible = ref(false)

</script>

<template>
  <span>OKINAWordle</span>
  <el-button type="default" :icon="QuestionFilled" circle plain @click="guideDialogVisible = true"></el-button>
  <el-dialog v-model="guideDialogVisible" title="HOW TO PLAY">
  </el-dialog>

  <el-button type="default" :icon="Setting" circle plain @click="settingDialogVisible = true"></el-button>
  <el-dialog v-model="settingDialogVisible" title="SETTING">
  </el-dialog>

  <el-button type="default" :icon="DataAnalysis" circle plain @click="statsDialogVisible = true"></el-button>
  <el-dialog v-model="statsDialogVisible" title="STATISTICS">
  </el-dialog>
</template>

<style scoped>
</style>
