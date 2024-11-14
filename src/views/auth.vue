<template>
  <div class="relative max-w-6xl px-4 mx-auto my-4">
    <div class="w-[320px] mx-auto mt-10">
      <el-input v-model="token" size="large" class="h-12" placeholder="请输入认证Token" />
      <el-button :loading="loading" class="w-full mt-4 lg:w-6xl" size="large" type="primary"
        @click="saveToken">保存</el-button>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ElInput, ElButton, ElMessage } from 'element-plus'
import storage from '../utils/storage'
import { ref } from 'vue'
import { useRouter } from 'vue-router'
import { checkToken } from '../utils/request'
const token = ref('')
const loading = ref(false)
const router = useRouter()
const saveToken = () => {
  loading.value = true
  if (!token.value) {
    loading.value = false
    ElMessage.error('请输入token')
    return
  }
  checkToken({
    token: token.value
  }).then(res => {
    if (res) {
      // 检测token是否有效
      storage.local.set('auth-token', token.value)
      router.push('/')
    } else {
      ElMessage.error('Token无效')
    }
  }).finally(() => {
    loading.value = false
  })

}
</script>

<style scoped>

</style>
