<template>
  <div class="home">
    <loading-icon v-if="Loading">
      <div class="loading-text">
        数据大屏加载中...
      </div>
    </loading-icon>
    <layout :options="{ width:3840, height:2160 }" v-else>
      <div class="test">
        imooc-container
      </div>
    </layout>
  </div>
</template>

<script>
import { onMounted, ref } from 'vue' 
import Layout from '@/components/Layout'
import LoadingIcon from '@/components/LoadingIcon'
export default {
  name: 'Home',
  components: {
    Layout,
    LoadingIcon
  },
  setup() {
    const Loading = ref(true)
    onMounted(() => {
      setTimeout(() => {
        Loading.value = false
      }, 2000)
    })
    return {
      Loading
    }
  }
}
</script>

<style lang="scss" scoped>
.home{
  position: relative;
  display: flex;
  align-items: center;
  justify-content: center;
  background: #000;
  width: 100%;
  height: 100%;
  font-size: 12px;
  .loading-text{
    margin-top: 10px;
  }
}
.test{
  color: red;
  font-size: 140px;
}
</style>
