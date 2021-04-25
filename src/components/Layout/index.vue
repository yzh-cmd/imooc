<template>
  <div class="imooc-layout" :ref="refName">
		<template v-if="ready">
			<slot></slot>
		</template>
  </div>
</template>

<script>
import { defineComponent, ref, onMounted, getCurrentInstance, onUnmounted, nextTick } from 'vue'
import { debounce } from '@/utils'
export default defineComponent({
	name: 'imooc-layout',
	props: {
		options: {
			type: Object
		}
	},
	setup(ctx) {
		const width = ref(0)	// 传入DOM的宽高
		const height = ref(0)
		const originalWidth = ref(0) // 实际视图的宽高
		const originalHeight = ref(0)
		const refName = 'imoocLayout'
		const ready = ref(false)
			// const Vue = require('vue')
		let Dom, observer
		const initSize = () => {
			return new Promise((resolve) => {
				nextTick(() => {
					if(ctx.options && ctx.options.width && ctx.options.height) {
					// 获取大屏的真实尺寸
						width.value = ctx.options.width
						height.value = ctx.options.height
					} else { // 如果没传入获取DOM的实际尺寸
						width.value = Dom.clientWidth
						height.value = Dom.clientHeight
					}
					// 获取画布的尺寸
					if(!originalWidth.value || !originalHeight.value) {
						originalWidth.value = window.screen.width
						originalHeight.value = window.screen.height
					}
					resolve()
				})
			})
		}
		// 更新DOM尺寸
		const updateSize = () => {
			if(!width.value && !height.value) {
				Dom.style.width = `${width.value}px`
				Dom.style.height = `${height.value}px`
			} else {
				Dom.style.width = `${originalWidth.value}px`
				Dom.style.height = `${originalHeight.value}px`
			}
		}
		
		const updateScale = () => {
			// 获取真实视口尺寸
			const currentWidth = document.body.clientWidth
			const currentHeight = document.body.clientHeight
			// 获取最终DOM尺寸
			const realWidth = width.value || originalWidth.value
			const realHeight = height.value || originalHeight.value

			const scaleWidth = currentWidth / realWidth
			const scaleHeight = currentHeight / realHeight
			Dom.style.transform = `scale(${scaleWidth}, ${scaleHeight})`
			console.log(width.value, height.value, originalWidth.value, originalHeight.value, currentWidth, currentHeight)
		}
		const onResize = async (e) => {
			console.log('e', e)
			await	initSize()
			updateScale()
		}
		const initMutationObserver = () => {
			const MutationObserver = window.MutationObserver
			observer = new MutationObserver(onResize)
			observer.observe(Dom, {
				attributes: true,
				attributeOldValue: true,
				attributeFilter: ['style']
			})
		}
		const offMutationObserver = () => {
			if(observer) {
				observer.disconnect()
				observer.takeRecords()
				observer = null
			}
		}
		onMounted(async() => {
			console.log(window)
			ready.value = false
			Dom = getCurrentInstance().refs[refName]
			await initSize()
			updateSize()
			updateScale()			
			window.addEventListener('resize', debounce(onResize, 100)) // 频繁操作DOM,可以使用防抖优化
			initMutationObserver()
			ready.value = true
		})
		onUnmounted(() => {
			window.removeEventListener('resize', onResize)
			offMutationObserver()
		})
		return {
			refName,
			ready
		}
	}
})
</script>

<style lang="scss" scoped>
.imooc-layout{
	position: fixed;
	top: 0;
	left: 0;
	width: 100% !important;
	height: 100% !important;
	overflow: hidden;
	transform-origin: top left;
	z-index: 999;
}
</style>
