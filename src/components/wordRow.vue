<script setup>
import { ref } from 'vue'
import tile from './tile.vue'
import { InfoFilled } from '@element-plus/icons-vue'

defineProps({
  wordData: Array,
  meanings: Array
})
const emit = defineEmits(['animation-end'])
const animationCatched = ref(0)

</script>

<template>
<div class="tilesWrapper">
<tile v-for="(letterData,index) in wordData" :letter="letterData.letter" :state="letterData.state" :index="index" :key="index" @animation-end="++animationCatched === 5 ? emit('animation-end') : null"></tile>
<el-popover v-if="meanings" placement="bottom" trigger="click" width="50vw">
  <template #reference>
    <el-icon color="black">
    <InfoFilled />
    </el-icon>
  </template>
  <div>
      答えの単語の意味
      <div v-for="meaning in meanings">・{{meaning}}</div>
  </div>
</el-popover>
<el-icon color="#D3D6DA" v-if="!meanings">
    <InfoFilled />
</el-icon>
</div>
</template>

<style scoped>
.tilesWrapper{
  display: flex;
  flex-wrap: wrap;
}

</style>
