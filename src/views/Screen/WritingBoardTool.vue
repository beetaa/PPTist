<template>
  <div class="writing-board-tool">
    <teleport to="body">
      <WritingBoard 
        ref="writingBoardRef" 
        :color="writingBoardColor" 
        :model="writingBoardModel" 
        v-if="writingBoardVisible" 
      />
    </teleport>

    <div class="tools">
      <div class="btn" @click="changePen()">画笔</div>
      <div class="btn" @click="changeEraser()">橡皮擦</div>
      <div class="btn" @click="clearCanvas()">擦除所有墨迹</div>
      <div class="btn" @click="closeWritingBoard()">关闭画笔</div>
      <div class="colors">
        <div 
          class="color" 
          :class="{ 'active': color === writingBoardColor }"
          v-for="color in writingBoardColors"
          :key="color"
          :style="{ backgroundColor: color }"
          @click="changeColor(color)"
        ></div>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent, ref } from 'vue'
import WritingBoard from '@/components/WritingBoard.vue'

const writingBoardColors = ['#000000', '#ffffff', '#1e497b', '#4e81bb', '#e2534d', '#9aba60', '#8165a0', '#47acc5', '#f9974c']

export default defineComponent({
  name: 'writing-board-tool',
  components: {
    WritingBoard,
  },
  setup(props, { emit }) {
    const writingBoardRef = ref()
    const writingBoardVisible = ref(false)
    const writingBoardColor = ref('#e2534d')
    const writingBoardModel = ref('pen')

    const changePen = () => {
      if(!writingBoardVisible.value) writingBoardVisible.value = true
      writingBoardModel.value = 'pen'
      emit('close')
    }

    const changeEraser = () => {
      writingBoardModel.value = 'eraser'
      emit('close')
    }

    const clearCanvas = () => {
      writingBoardRef.value.clearCanvas()
      emit('close')
    }

    const changeColor = (color: string) => {
      if(writingBoardModel.value !== 'pen') writingBoardModel.value = 'pen'
      writingBoardColor.value = color
      emit('close')
    }

    const closeWritingBoard = () => {
      writingBoardVisible.value = false
      emit('close')
    }

    return {
      writingBoardRef,
      writingBoardVisible,
      writingBoardColors,
      writingBoardColor,
      writingBoardModel,
      changePen,
      changeEraser,
      clearCanvas,
      changeColor,
      closeWritingBoard,
    }
  },
})
</script>

<style lang="scss" scoped>
.writing-board-tool {
  font-size: 12px;

  .btn {
    padding: 3px 10px;
    margin: 0 -10px;
    margin-bottom: 3px;
    cursor: pointer;

    &:hover {
      background-color: #ccc;
    }
  }
  .colors {
    display: flex;
    margin-top: 8px;
  }
  .color {
    width: 15px;
    height: 15px;
    outline: 1px solid #ccc;
    cursor: pointer;

    &:hover {
      transform: scale(1.1);
    }
    &.active {
      outline: 2px solid $themeColor;
    }

    & + .color {
      margin-left: 5px;
    }
  }
}
</style>