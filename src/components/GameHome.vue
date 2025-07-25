<template>
  <div class="game-home">
    <div class="game-home-title">
      <div>重生拆卡</div>
      <div class="game-home-tag">@muzikikoo</div>
      <div>模拟器</div>
    </div>
    <div class="game-home-button-group">
      <div><el-button class="game-home-button" @click="startGame">开始游戏</el-button></div>
      <div><el-row class="space-row" /></div>
      <div><el-button class="game-home-button" @click="importRecord">导入存档</el-button></div>
      <div><el-row class="space-row" /></div>
      <div><el-button class="game-home-button" @click="changeSettings">其他设置</el-button></div>
    </div>
  </div>
  <el-dialog v-model="dialogVisible" class="game-home-dialog" :style="dialogStyle">
    <div class="dialog-msg">{{ dialogMsg }}</div>
    <template #footer>
      <div class="dialog-footer">
        <el-button
          class="dialog-button"
          @click="dialogVisible = false"
          :disabled="!CheckLocalStorage()"
          >读取存档</el-button
        >
        <el-button class="dialog-button" @click="newGame">全新开始</el-button>
      </div>
    </template>
  </el-dialog>
</template>

<script setup lang="ts">
import { ref, computed } from 'vue'
import { ElButton, ElRow, ElDialog } from 'element-plus'
import 'element-plus/es/components/button/style/css'
import 'element-plus/es/components/row/style/css'
import 'element-plus/es/components/dialog/style/css'

const emit = defineEmits(['goPage'])

const dialogVisible = ref(false)
const dialogMsg = ref('识别本地存档中...')
const dialogStyle = computed(() => {
  return window.innerWidth < 1024
    ? {
        width: '30rem',
        height: '18.75rem',
        top: '25vh',
        backgroundColor: 'var(--theme-grey-grey)',
        '--dialog-border-color': 'var(--theme-deep-green)',
      }
    : {
        width: '30rem',
        height: '18.75rem',
        top: '25vh',
        backgroundColor: 'var(--theme-grey-grey)',
        '--dialog-border-color': 'var(--theme-deep-green)',
      }
})

const CheckLocalStorage = () => {
  console.log('WARNING:识别本地存档功能未完成！')
  return false
}

function openDialog() {
  dialogVisible.value = true
  const localStorageCheck = CheckLocalStorage()
  if (localStorageCheck) {
    dialogMsg.value = '识别到本地有存档！'
  } else {
    dialogMsg.value = '识别到本地没有存档！'
  }
}

function startGame() {
  openDialog()
}

function newGame() {
  dialogVisible.value = false
  emit('goPage', 'main')
}

function importRecord() {
  console.log('WARNING:导入存档功能未完成！')
}

function changeSettings() {
  console.log('WARNING:其他设置功能未完成！')
}
</script>

<style scoped>
.game-home {
  justify-items: center;
}
.game-home-title {
  position: fixed;
  top: 15%;
  margin-left: 5%;
  text-align: center;
  font-size: 6rem;
  letter-spacing: 3rem;
  color: var(--theme-flame);
  -webkit-text-stroke: 0.125rem var(--theme-grey-grey);
  -webkit-text-fill-color: var(--theme-flame);
  text-shadow:
    -0.125rem -0.125rem 0 var(--theme-grey-grey),
    0.125rem -0.125rem 0 var(--theme-grey-grey),
    -0.125rem 0.125rem 0 var(--theme-grey-grey),
    0.125rem 0.125rem 0 var(--theme-grey-grey);
}
.game-home-tag {
  font-size: 1.5rem;
  letter-spacing: 0.5rem;
  -webkit-text-stroke: 0;
  -webkit-text-fill-color: var(--theme-grey-grey);
}
.game-home-button-group {
  position: fixed;
  top: 50%;
  justify-items: center;
  padding: 2rem;
}
.game-home-button {
  font-size: 2.5rem;
  letter-spacing: 1.125rem;
  height: 9vh;
  width: 50vw;
  background-color: var(--theme-deep-green);
  color: white;
  &:hover {
    background-color: var(--theme-deep-green-hover);
  }
}
.space-row {
  height: 4vh;
}
:deep(.el-button) {
  border-radius: 1.25rem;
  border: 0;
  box-shadow: 0 0.25rem 0.25rem rgba(0, 0, 0, 0.25);
  &:hover {
    box-shadow: 0 0.375rem 0.5rem rgba(0, 0, 0, 0.3);
  }
}

.dialog-msg {
  font-size: 2rem;
  padding: 2rem;
  letter-spacing: 0.125rem;
}
.dialog-footer {
  text-align: center;
}

.dialog-button {
  background-color: var(--theme-deep-green);
  color: white;
  height: 4rem;
  font-size: 1.75rem;
  letter-spacing: 0.375rem;
}
.dialog-button:disabled {
  background-color: var(--theme-deep-green-disabled);
}

@media (min-width: 1024px) {
  .game-home-title {
    position: fixed;
    margin-left: 3%;
    top: 8%;
    text-align: center;
    font-size: 4.5rem;
    letter-spacing: 2.25rem;
    color: var(--theme-flame);
    -webkit-text-stroke: 0.125rem var(--theme-grey-grey);
    -webkit-text-fill-color: var(--theme-flame);
    text-shadow:
      -0.125rem -0.125rem 0 var(--theme-grey-grey),
      0.125rem -0.125rem 0 var(--theme-grey-grey),
      -0.125rem 0.125rem 0 var(--theme-grey-grey),
      0.125rem 0.125rem 0 var(--theme-grey-grey);
  }
  .game-home-button {
    font-size: 2rem;
    letter-spacing: 1rem;
    background-color: var(--theme-deep-green);
    color: white;
    &:hover {
      background-color: var(--theme-deep-green-hover);
    }
  }
  .dialog-msg {
    font-size: 2rem;
    padding: 2rem;
  }
}
</style>
