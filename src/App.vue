<template>
	<el-config-provider :locale="zhCn">
		<div class="w-full h-screen overflow-x-hidden overflow-y-auto">
			<el-scrollbar>
				<div class="sticky top-0 left-0 z-10 w-full h-16 shadow-sm bg-gray-50 backdrop-blur-sm">
					<div class="flex items-center h-full max-w-6xl px-4 mx-auto">
						<img src="/favicon.png" class="block w-8 h-8 mr-2" />
						<div class="text-lg">
							{{ appName }}
						</div>
						<div class="flex-1"></div>

						<div :class="{
							'bg-gray-100': $route.path === '/up'
						}" class="px-3 py-2 mr-2 text-sm text-gray-800 rounded-md cursor-pointer" @click="router.push('/up')">
							<font-awesome-icon :icon="faUpload" :class="[$route.path === '/up' ? 'text-white' : 'text-gray-500']" />
							<span class="hidden pl-2 md:inline-block">上传</span>
						</div>

						<div :class="{
							'bg-gray-100': $route.path === '/'
						}" class="px-3 py-2 text-sm text-gray-800 rounded-md cursor-pointer" @click="router.push('/')">
							<font-awesome-icon :icon="faCog" :class="[$route.path === '/' ? 'text-white' : 'text-gray-500']" />
							<span class="hidden pl-2 md:inline-block">管理</span>
						</div>
					</div>
				</div>

				<div class="min-h-[calc(100vh-64px-64px)] overflow-auto">
					<router-view />
				</div>

				<div class="flex items-center justify-center w-full h-16 text-sm text-gray-500">
					<a :href="repoLink" target="_blank" class="underline">
						{{ repoName }}
					</a>
				</div>
			</el-scrollbar>
		</div>
	</el-config-provider>
</template>

<script setup lang="ts">
import { faCog, faUpload } from '@fortawesome/free-solid-svg-icons'
import { useRouter } from 'vue-router'
import { ElScrollbar, ElConfigProvider } from 'element-plus'
import zhCn from "element-plus/es/locale/lang/zh-cn"

const appName = '🔥 火舞木木 - 图床'

document.title = appName

const router = useRouter()
</script>
