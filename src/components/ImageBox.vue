<template>
	<div class="relative w-full overflow-hidden rounded-md shadow-sm bg-rose-100">
		<loading-overlay :loading="loading" />

		<el-image class="block w-full h-40 lg:h-60" :src="copyUrl" fit="cover" hide-on-click-modal
			@load="loading = false" />
		<div class="absolute bottom-0 left-0 w-full bg-slate-800/70 backdrop-blur-sm">
			<div class="p-2">
				<div class="flex items-center w-full text-white">
					<div class="flex-1 w-full truncate">
						<el-tooltip :content="name" placement="top-start">
							{{ name }}
						</el-tooltip>
					</div>
					<div v-if="mode === 'converted'" class="flex items-center justify-center w-6 h-6 cursor-pointer"
						@click="emit('delete')">
						<font-awesome-icon :icon="faTimesCircle" />
					</div>
				</div>
				<span class="flex items-center text-xs text-gray-300">
					{{ formatBytes(size) }}
					<el-divider v-if="uploadedAt" direction="vertical" />
					<span v-if="uploadedAt">
						{{ new Date(uploadedAt).toLocaleDateString() }}
					</span>
				</span>
			</div>
			<div v-if="mode === 'uploaded'">
				<el-divider class="m-0" />
				<div class="flex w-full text-sm text-center text-white h-9">
					<el-tooltip :content="copyUrl" placement="top-start">
						<div class="flex items-center justify-center flex-1 cursor-pointer" @click="copyLink(copyUrl)">
							<font-awesome-icon :icon="faCopy" class="mr-2" />
							链接
						</div>
					</el-tooltip>
					<el-divider direction="vertical" class="h-full" />
					<el-popconfirm title="确认删除图片吗？" confirm-button-type="danger" @confirm="
							() => {
								// (e: Event) => boolean ???
								loading = true
								emit('delete')
								return true
							}
						">
						<template #reference>
							<div class="flex items-center justify-center flex-1 cursor-pointer">
								<font-awesome-icon :icon="faTrashAlt" class="mr-2" />
								删除
							</div>
						</template>
					</el-popconfirm>
				</div>
			</div>
		</div>
	</div>
</template>

<script setup lang="ts">
import { faTimesCircle, faTrashAlt, faCopy } from '@fortawesome/free-regular-svg-icons'
import copy from 'copy-to-clipboard'
import formatBytes from '../utils/format-bytes'
import {ElTooltip, ElDivider, ElPopconfirm, ElImage, ElMessage} from 'element-plus'
import { ref } from 'vue'
import LoadingOverlay from '../components/LoadingOverlay.vue'

const props = defineProps<{
	src: string
	copyUrl:string
	name: string
	size: number
	mode: 'converted' | 'uploaded'
	uploadedAt?: number
	expiresAt?: number
}>()
const emit = defineEmits(['delete'])

const imageError = ref(false)
const loading = ref(true)
const copyLink = (link : string) => {
  const res = copy(link)
  if (res) {
    ElMessage.success('链接复制成功')
  } else {
    ElMessage.success('链接复制失败')
  }
}
</script>
