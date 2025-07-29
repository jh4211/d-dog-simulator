<script setup lang="ts">
import { ref, reactive, computed, onMounted } from 'vue'
import { ElButton, ElDialog, ElForm, ElFormItem, ElInput } from 'element-plus'
// 1. 直接导入所有头像图片（根据实际文件名修改）
import avatar1 from '@/assets/avatar/avatar1.png'
import avatar2 from '@/assets/avatar/avatar2.png'
import avatar3 from '@/assets/avatar/avatar3.png'
import avatar4 from '@/assets/avatar/avatar4.png'
import avatar5 from '@/assets/avatar/avatar5.png'
import avatar6 from '@/assets/avatar/avatar6.png'

// 导入样式（保持不变）
import 'element-plus/es/components/button/style/css'
import 'element-plus/es/components/dialog/style/css'
import 'element-plus/es/components/form/style/css'
import 'element-plus/es/components/form-item/style/css'
import 'element-plus/es/components/input/style/css'

// 2. 存储导入的头像图片（直接使用导入的变量）
const avatarList = [avatar1, avatar2, avatar3, avatar4, avatar5, avatar6]

const emit = defineEmits(['goPage'])
const dialogVisible = ref(true)
const formRef = ref<InstanceType<typeof ElForm>>()
const form = reactive({
  name: '',
  amount: '1000',
  avatar: avatar1, // 初始值设为第一张，后续会被随机覆盖
})

const rules = reactive({
  name: [
    { required: true, message: '请输入玩家名称', trigger: 'blur' },
    { min: 2, max: 4, message: '名称长度在2-4个字符', trigger: 'blur' },
  ],
})

// 随机选择头像的方法（直接从导入的图片数组中选）
const getRandomAvatar = () => {
  const randomIndex = Math.floor(Math.random() * avatarList.length)
  return avatarList[randomIndex]
}

// 更换头像
const changeAvatar = () => {
  form.avatar = getRandomAvatar()
}

// 初始化时随机选择一个头像
onMounted(() => {
  form.avatar = getRandomAvatar()
})

const dialogStyle = computed(() => ({
  width: window.innerWidth < 1024 ? '75vw' : '270px',
  height: window.innerWidth < 1024 ? '75vh' : '480px',
  backgroundColor: 'var(--theme-grey-grey)',
  '--dialog-border-color': 'var(--theme-flame)',
}))

function createPlayer() {
  formRef.value?.validate((valid) => {
    if (valid) {
      dialogVisible.value = false
      emit('goPage', {
        name: form.name,
        amount: form.amount,
        avatar: form.avatar, // 传递的是导入的图片对象（在父组件中可直接作为src使用）
      })
    }
  })
}
</script>

<template>
  <div class="game-play-page">
    <el-dialog
      v-model="dialogVisible"
      class="game-home-dialog"
      :style="dialogStyle"
      custom-class="center-dialog"
      :show-close="false"
      :close-on-click-modal="false"
      :close-on-press-escape="false"
    >
      <template #header>
        <div class="dialog-header">创建角色</div>
      </template>
      <div>
        <el-form class="custom-from" ref="formRef" :model="form" :rules="rules" label-width="auto">
          <!-- 头像选择项 -->
          <el-form-item label="玩家头像">
            <div class="avatar-container">
              <!-- 3. 直接使用导入的图片对象作为src -->
              <div class="avatar-preview">
                <img :src="form.avatar" alt="玩家头像" class="avatar-img" />
              </div>
              <el-button size="small" type="text" class="change-avatar-btn" @click="changeAvatar">
                换一个
              </el-button>
            </div>
          </el-form-item>

          <el-form-item label="玩家名称" prop="name">
            <el-input v-model="form.name" placeholder="请输入玩家名称" />
          </el-form-item>

          <el-form-item label="初始资金" prop="amount">
            <el-input v-model="form.amount" disabled />
          </el-form-item>
        </el-form>
      </div>
      <template #footer>
        <div class="dialog-footer">
          <el-button class="dialog-button" @click="createPlayer">开始游戏</el-button>
        </div>
      </template>
    </el-dialog>
  </div>
</template>

<style scoped>
/* 样式保持不变 */
.game-play-page {
  display: flex;
  justify-content: center;
  align-items: center;
  min-height: 100vh;
  width: 100vw;
  height: 100vh;
  margin: 0;
  padding: 0;
  background-image: url('@/assets/pic/mainpage.png');
  background-size: cover;
  background-position: center;
  background-repeat: no-repeat;
  background-attachment: fixed;
}

.center-dialog {
  display: flex !important;
  flex-direction: column;
  margin: 0 !important;
}

.center-dialog .el-dialog__wrapper {
  display: flex;
  justify-content: center;
  align-items: center;
}

.dialog-header {
  color: var(--el-text-color-regular);
  font-size: 20px;
  letter-spacing: 4px;
  text-align: center;
}

.dialog-footer {
  text-align: center;
  align-items: center;
}

.dialog-button {
  background-color: var(--theme-flame);
  color: white;
  height: 40px;
  font-size: 18px;
  letter-spacing: 2px;
}

.custom-from {
  padding: 10px;
}

:deep(.el-form-item__label) {
  font-size: 16px;
}

.avatar-container {
  display: flex;
  flex-direction: column;
  align-items: center;
  margin-top: 5px;
}

.avatar-preview {
  width: 72px;
  height: 72px;
  background-color: var(--theme-dark);
  border-radius: 50%;
  overflow: hidden;
  margin-bottom: 10px;
  border: 2px solid #ddd;
  display: flex;
  justify-content: center;
  align-items: center;
}

.avatar-img {
  width: 80%;
  height: 80%;
  object-fit: cover;
}

.change-avatar-btn {
  color: var(--theme-flame);
  padding: 5px 0;
  font-size: 14px;
}

:deep(.el-form-item) {
  display: flex;
  align-items: center; /* 垂直居中核心 */
  margin-bottom: 15px; /* 调整表单项间距，避免拥挤 */
}

:deep(.el-form-item__label) {
  font-size: 16px;
  white-space: nowrap; /* 防止标签文字换行 */
  margin-right: 10px; /* 标签与内容的间距 */
  padding: 0; /* 清除默认内边距 */
}
</style>
