<script setup lang="ts">
import { ref, computed } from 'vue'
import { ElButton, ElRow, ElDialog } from 'element-plus'
import 'element-plus/es/components/button/style/css'
import 'element-plus/es/components/row/style/css' // ElRow 样式
import 'element-plus/es/components/dialog/style/css' // ElDialog 样式

const emit = defineEmits(['goPage'])

const dialogVisible = ref(false)
const dialogMsg = ref('识别本地存档中...')
const dialogStyle = computed(() => {
  return window.innerWidth < 1024
    ? {
        width: '90vw',
        height: '180px',
        top: '25vh',
        backgroundColor: 'var(--theme-grey-grey)',
        '--dialog-border-color': 'var(--theme-deep-green)',
      }
    : {
        width: '480px',
        height: '200px',
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

<style scoped>
.game-home {
  justify-items: center;
}
.game-home-title {
  width: 90vw;
  position: fixed;
  top: 8%;
  /* 关键：水平居中核心属性 */
  left: 50%; /* 相对于父容器左移50% */
  transform: translateX(-50%); /* 向左偏移自身宽度的50%，实现真正居中 */
  text-align: center; /* 文字内容在标题内部居中 */

  /* 移除导致偏移的 margin-left */
  margin-left: 0; /* 清除原有的 margin-left: 5% 或 3% */

  font-size: 60px;
  letter-spacing: 8px;
  color: var(--theme-flame);
  -webkit-text-stroke: 2px var(--theme-grey-grey);
  -webkit-text-fill-color: var(--theme-flame);
  text-shadow:
    -2px -2px 0 var(--theme-grey-grey),
    2px -2px 0 var(--theme-grey-grey),
    /* 修复原样式中的笔误：-2x → -2px */ -2px 2px 0 var(--theme-grey-grey),
    2px 2px 0 var(--theme-grey-grey);
}
.game-home-tag {
  font-size: 16px;
  letter-spacing: 4px;
  -webkit-text-stroke: 0px;
  -webkit-text-fill-color: var(--theme-grey-grey);
  /* 关键：清除文字阴影 */
  text-shadow: none; /* 覆盖继承的阴影样式 */
}
.game-home-button-group {
  position: fixed;
  top: 50%;
  /* 关键：添加 Grid 布局 */
  display: grid;
  justify-items: center; /* 子元素水平居中 */
  width: 100%; /* 确保容器占满屏幕宽度，否则无法居中 */
  padding: 2rem;
  box-sizing: border-box; /* 避免 padding 导致容器溢出 */
}
.game-home-button {
  font-size: 24px;
  letter-spacing: 8px;
  height: 9vh;
  margin: 0 auto;
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
  border-radius: 20px;
  border: 0px;
  /* 添加阴影效果 */
  box-shadow: 0 4px 4px rgba(0, 0, 0, 0.25); /* x0 y4 blur4 */

  /* 悬停时阴影加深 */
  &:hover {
    box-shadow: 0 6px 8px rgba(0, 0, 0, 0.3); /* 可选：悬停时增强阴影 */
  }
}

.dialog-msg {
  font-size: 20px;
  padding: 5px;
  letter-spacing: 2px;
}
.dialog-footer {
  text-align: center;
}

.dialog-button {
  background-color: var(--theme-deep-green);
  color: white;
  height: 40px;
  font-size: 18px;
  letter-spacing: 2px;
}
.dialog-button:disabled {
  background-color: var(--theme-deep-green-disabled);
}

@media (min-width: 1024px) {
  .game-home-title {
    top: 8%;
    /* 保持水平居中逻辑 */
    left: 50%;
    transform: translateX(-50%);
    margin-left: 0; /* 清除桌面端的 margin-left: 3% */

    font-size: 72px;
    letter-spacing: 36px;
  }
  .game-home-button {
    font-size: 32px;
    letter-spacing: 16px;
    background-color: var(--theme-deep-green);
    color: white;
    &:hover {
      background-color: var(--theme-deep-green-hover);
    }
  }
  .dialog-msg {
    font-size: 32px;
    padding: 5px;
  }
}
</style>
