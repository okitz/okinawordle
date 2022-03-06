<script setup>
import { ref } from 'vue'
import tile from './tile.vue'
import { InfoFilled } from '@element-plus/icons-vue'

const props = defineProps({
  wordData: Array,
  meanings: Array,
  LENGTH: Number
})
const emit = defineEmits(['animation-end'])
const animationCatched = ref(0)

</script>

<template>
<div class="tilesWrapper">
<tile v-for="(letterData,index) in wordData" :letter="letterData.letter" :state="letterData.state" :index="index" :key="index" @animation-end="++animationCatched === props.LENGTH ? emit('animation-end') : null"></tile>
<el-popover v-if="meanings" placement="bottom" trigger="click" width="min(85%,40rem)">
  <template #reference>
    <el-icon color="black">
    <InfoFilled />
    </el-icon>
  </template>
  <div>
      単語の意味
      <div v-for="meaning in meanings" style="margin: 0.4rem;">{{meaning}}</div>
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
