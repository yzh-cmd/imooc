<template>
  <div class="loading-warpper">
		<svg
			:width="width"
			:height="height"
			viewBox="0 0 50 50"
			preserveAspectRatio="xMidYMid meet"
		>
			<circle
				cx="25"
				cy="25"
				r="22"
				:stroke="outSideColor"
				stroke-width="3"
				stroke-linecap="round"
				stroke-dasharray="35"
				fill="transparent"
			>
				<animateTransform
					attributeName="transform"
					type="rotate"
					from="0 25 25"
					to="360 25 25"
					dur="2s"
					repeatCount="indefinite"
				/>
				<animate
					attributeName="stroke"
					:values="outSideColorAnimation"
					:dur="duration"
					repeatCount="indefinite"
				/>
			</circle>
			<circle
				cx="25"
				cy="25"
				r="12"
				stroke-width="3"
				:stroke="inSideColor"
				stroke-linecap="round"
				stroke-dasharray="19"
				fill="transparent"
			>
				<animateTransform
					attributeName="transform"
					type="rotate"
					from="360 25 25"
					to="0 25 25"
					dur="2s"
					repeatCount="indefinite"
				/>
				<animate
					attributeName="stroke"
					:values="inSideColorAnimation"
					:dur="duration"
					repeatCount="indefinite"
				/>
			</circle>
		</svg>
		<slot></slot>
  </div>
</template>

<script>
import { computed, defineComponent } from 'vue'
export default defineComponent({
	name: 'LoadingIcon',
	props: {
		width: {
			type: [Number, String],
			default: 50
		},
		height: {
			type: [Number, String],
			default: 50
		},
		outSideColor: {
			type: [Number, String],
			default: '#04bdfd'
		},
		inSideColor: {
			type: [Number, String],
			default: '#39e5cc'
		},
		duration: {
			type: String,
			default: '2s'
		}
	},
	setup(ctx) {
		const outSideColorAnimation = computed(() => `${ctx.outSideColor};${ctx.inSideColor};${ctx.outSideColor}`)
		const inSideColorAnimation = computed(() => `${ctx.inSideColor};${ctx.outSideColor};${ctx.inSideColor}`)
		return {
			outSideColorAnimation,
			inSideColorAnimation
		}
	}
}) 
</script>

<style>
.loading-warpper{
	width: 200px;
	height: 200px;
	display: flex;
	justify-content: center;
	align-items: center;
	color: #fff;
	flex-direction: column;
}
</style>
