<template>
  <el-container>
    <el-header class="header">
      <div style="margin-top: 15px;">
        <div style="width: fit-content;float: left;">
          <svg style="display:inline-block;vertical-align: -12px;width: 40px;height: 40px;" t="1690001220506" class="icon"
            viewBox="0 0 1024 1024" version="1.1" xmlns="http://www.w3.org/2000/svg" p-id="2501" width="30" height="30">
            <path
              d="M484.266667 718.933333c-27.733333 0-53.333333-10.666667-74.666667-29.866666-21.333333-19.2-32-46.933333-32-74.666667 0-27.733333 10.666667-55.466667 29.866667-76.8 34.133333-36.266667 198.4-102.4 232.533333-115.2 8.533333-2.133333 17.066667-2.133333 23.466667 4.266667 6.4 6.4 8.533333 14.933333 4.266666 23.466666-12.8 34.133333-72.533333 200.533333-106.666666 236.8-21.333333 19.2-46.933333 32-76.8 32z m125.866666-236.8c-70.4 29.866667-155.733333 68.266667-172.8 85.333334-12.8 12.8-19.2 27.733333-17.066666 44.8 0 17.066667 6.4 32 19.2 44.8 12.8 12.8 27.733333 17.066667 44.8 17.066666s32-6.4 44.8-19.2c17.066667-17.066667 53.333333-102.4 81.066666-172.8z"
              fill="#05f0f9" p-id="2502"></path>
            <path
              d="M196.266667 891.733333c-6.4 0-10.666667-2.133333-14.933334-6.4C91.733333 797.866667 42.666667 680.533333 42.666667 554.666667 42.666667 296.533333 253.866667 85.333333 512 85.333333s469.333333 211.2 469.333333 469.333334c0 125.866667-49.066667 243.2-136.533333 332.8-8.533333 8.533333-21.333333 8.533333-29.866667 0s-8.533333-21.333333 0-29.866667C893.866667 776.533333 938.666667 667.733333 938.666667 554.666667c0-234.666667-192-426.666667-426.666667-426.666667S85.333333 320 85.333333 554.666667c0 113.066667 44.8 221.866667 125.866667 300.8 8.533333 8.533333 8.533333 21.333333 0 29.866666-4.266667 6.4-10.666667 6.4-14.933333 6.4z"
              fill="#05f0f9" p-id="2503"></path>
            <path
              d="M512 234.666667c-12.8 0-21.333333-8.533333-21.333333-21.333334V128c0-12.8 8.533333-21.333333 21.333333-21.333333s21.333333 8.533333 21.333333 21.333333v85.333333c0 12.8-8.533333 21.333333-21.333333 21.333334zM761.6 356.266667c-6.4 0-12.8-2.133333-17.066667-8.533334-6.4-8.533333-6.4-23.466667 2.133334-29.866666l66.133333-53.333334c8.533333-6.4 23.466667-6.4 29.866667 2.133334 6.4 8.533333 6.4 23.466667-2.133334 29.866666l-66.133333 53.333334c-2.133333 4.266667-6.4 6.4-12.8 6.4zM906.666667 644.266667h-4.266667l-83.2-19.2c-10.666667-2.133333-19.2-14.933333-17.066667-25.6 2.133333-10.666667 14.933333-19.2 25.6-17.066667l83.2 19.2c10.666667 2.133333 19.2 14.933333 17.066667 25.6-2.133333 10.666667-10.666667 17.066667-21.333333 17.066667zM117.333333 644.266667c-10.666667 0-19.2-6.4-21.333333-17.066667-2.133333-10.666667 4.266667-23.466667 14.933333-25.6l83.2-19.2c10.666667-2.133333 23.466667 4.266667 25.6 17.066667 2.133333 10.666667-4.266667 23.466667-14.933333 25.6l-83.2 19.2h-4.266667zM262.4 356.266667c-4.266667 0-8.533333-2.133333-12.8-4.266667L183.466667 298.666667c-8.533333-6.4-10.666667-21.333333-2.133334-29.866667 6.4-8.533333 21.333333-10.666667 29.866667-2.133333L277.333333 320c8.533333 6.4 10.666667 21.333333 2.133334 29.866667-6.4 2.133333-10.666667 6.4-17.066667 6.4z"
              fill="#05f0f9" p-id="2504"></path>
          </svg>
          <span class="title"> 网络面板 </span>
        </div>
        <div style="float: right;">
          <el-button class="home" @click="GoToHomePage" round>Q群</el-button>
        </div>
      </div>
    </el-header>
    <el-main>
      <MainUI :isVisible="isVisible" :IPinfo="IPinfo" />
      <br>
      <IPinfoUI :isVisible="isVisible" :IPinfo="IPinfo" />
    </el-main>
    <div style="height: fit-content;padding-bottom: 10px;">
      <div style="width: fit-content;margin-left: auto;margin-right: auto;">
        <el-link type="info" href="https://netart.cn/" target="_blank">Whoami</el-link>&nbsp;
        <el-link type="info" href="https://github.com/ljxi/NetworkPanel" target="_blank">源码</el-link>
      </div>
    </div>
  </el-container>
</template>

<script lang="ts" setup>
// import { useDark, useToggle } from '@vueuse/core'

// const isDark = useDark()
// const toggleDark = useToggle(isDark)
// toggleDark()
import MainUI from "./components/Main.vue"
import IPinfoUI from "./components/IPinfo.vue"
import { ref, reactive } from 'vue'
import { ElMessage } from 'element-plus'
import { toClipboard } from '@soerenmartius/vue3-clipboard'

const isVisible = ref(true)
const IPinfo = reactive({ globalInfo: null, localInfo: null })
let GoToHomePage = () => {
  //window.open("https://netart.cn/")
  toClipboard('463481772')
  ElMessage.info({
    dangerouslyUseHTMLString: true,
    message: '<center>已经复制QQ群号：<br><strong>463481772</strong></center>',
  })
}
document.addEventListener("visibilitychange", function () {
  var string = document.visibilityState
  if (string === 'hidden') isVisible.value = false
  else isVisible.value = true
})
</script>

<style>
.header {
  height: fit-content;
  padding-bottom: 12px;
  background-color: #ffffff;
}

.title{
  color: #526484;
  font-size: 20px;
  font-weight: bolder;
}
.home{
  color: #526484;
  font-size: 12px;
}
@media (prefers-color-scheme: dark) {
  .header {
    background-color: rgb(18,18,18);
  }
  .title{
    color: rgb(152,167,202);
  }
  .home{
    color: rgb(152,167,202);
  }
}</style>