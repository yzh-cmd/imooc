<template>
  <div class="FlyLineBox" ref="FlyBoxDom">
		<svg :width="width" :height="height">
			<defs>
        <path 
          :id="pathId" 
          :d="path"
          fill="none"
        />
        <mask :id="maskId">
          <circle
            cx="0"
            cy="0"
            r="50"
            :fill="`url(#${radialGradientId})`"
          >
            <animateMotion 
              :path="path"
              :stroke="starColor"
              rotate="auto"
              dur="3s"
              repeatCount="indefinite"
            />
          </circle>
        </mask>
        <radialGradient 
          :id="radialGradientId"
          cx="50%"
          cy="50%"
          fx="100%"
          fy="50%"
          r="50%"
          >
          <stop offset="0%" stop-color="#fff" stop-opacity="1"></stop> 
          <stop offset="100%" stop-color="#fff" stop-opacity="0"></stop>
        </radialGradient>
      </defs>
      <use 
        :href="`#${pathId}`"
        stroke-width="1"
        :stroke="lineColor"
      />
      <use 
        :href="`#${pathId}`" 
        stroke-width="3"
        stroke="#4fd2dd"
        :mask="`url(#${maskId})`"
      />
		</svg>
    <div class="fly-box-content">
      <slot></slot>
    </div>
  </div>
</template>

<script>
import { defineComponent, getCurrentInstance, ref, onMounted, computed } from 'vue' 
import { v4 as uuidv4 } from 'uuid'
export default defineComponent({
  name: 'FlyLineBox',
  props: {
    lineColor: {
      type: String,
      default: '#235fa7'
    },
    starColor: {
      type: String,
      default: '#4fd2dd'
    }
  },
  setup() {
    let width = ref(0)
    let height = ref(0)
    const refName = 'FlyBoxDom'
    const uuid = uuidv4()
    const pathId = `${refName}-pathId-${uuid}` 
    const maskId = `${refName}-maskId-${uuid}` 
    const radialGradientId = `${refName}-radialGradient-${uuid}` 
    onMounted(() => {
      const FlyBoxDom = getCurrentInstance().refs[refName]
      width.value = FlyBoxDom.clientWidth
      height.value = FlyBoxDom.clientHeight
    })
    const path = computed(() => {
      return `M5 5 L${width.value - 5} 5 L${width.value - 5} ${height.value - 5} L5 ${height.value - 5} Z`
    })
    return {
      width,
      height,
      path,
      pathId,
      maskId,
      radialGradientId
    }
  }
})
</script>

<style>
.FlyLineBox{
  margin: 0 auto;
  width: 100%;
  height: 100%;
  position: relative;
}
.FlyLineBox > svg{
  position: absolute;
  width: 100%;
  height: 100%;
  top: 0;
  left: 0;
}
.fly-box-content{
  width: 100%;
  height: 100%;
  padding: 5px;
  box-sizing: border-box;
}
</style>