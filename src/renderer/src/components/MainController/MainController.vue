<template>
  <div ref="draggableRef" class="draggable">
    <div class="main-controller">
      <span @click="resize('reload')">
        <el-icon><Refresh /></el-icon>
      </span>
      <span @click="resize('minimize')">
        <el-icon><Minus /></el-icon>
      </span>
      <span v-if="!isMax" @click="resize('maximize')">
        <el-icon><FullScreen /></el-icon>
      </span>
      <span v-if="isMax" @click="resize('restore')">
        <el-icon><Connection /></el-icon>
      </span>
      <span class="last" @click="resize('close')">
        <el-icon><Close /></el-icon>
      </span>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue'
import useDrag from '@renderer/hooks/useDrag'

const draggableRef = useDrag()

const isMax = ref(false)
function resize(type: string) {
  window.electron.ipcRenderer.send('resize', type)
}
window.onresize = async () => {
  isMax.value = await window.electron.ipcRenderer.invoke('isMaximized')
}
</script>

<style lang="less" scoped>
.draggable {
  width: 100%;
  height: 35px;
  position: absolute;
  top: 0;
  z-index: 999;
}

.main-controller {
  -webkit-app-region: drag;
  -webkit-user-select: none;
  position: absolute;
  top: 0;
  right: 0;
  line-height: 35px;
  display: flex;
  justify-content: space-between;
  z-index: 999;
  span {
    cursor: pointer;
    -webkit-app-region: no-drag;
    display: block;
    width: 50px;
    text-align: center;
    &:hover {
      background-color: #ccc;
    }
    &:last-child:hover {
      background-color: #e81123;
      color: #fff;
    }
  }
}
</style>
