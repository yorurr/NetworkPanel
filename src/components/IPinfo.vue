<template>
    <div class="radius card"
          :style="{borderRadius:`var(--el-border-radius-round)`}">
        <div style="text-align: center;">
          <transition name="el-fade-in">
            <div v-loading="!info.localInfo" v-if="!info.localInfo || info.localInfo['isChinaMainland']" >
                <el-tooltip class="item" effect="dark" :content="info.localInfo?info.localInfo['ip']:'Loading...'" placement="top">
                    <div @click="copy(info.localInfo?info.localInfo['ip']:'')">
                        <el-tag style="width: 50px;" class="ml-2" type="success">{{ info.localLay?info.localLay+"ms":"-ms" }}</el-tag>
                        <el-text style="cursor: pointer;margin-left: 5px;white-space:nowrap;vertical-align: -1px;" class="font-background">{{ info.localInfo?info.localInfo['province'] + ' ' + info.localInfo['city'] + ' ' + info.localInfo['area'] + ' ' + info.localInfo['isp']:"Loading..." }}</el-text>
                    </div>
                </el-tooltip>
            </div>
          </transition>
          <transition name="el-fade-in">
              <div v-loading="!info.globalInfo" v-if="(info.localInfo && info.localInfo['province'] && !info.globalInfo) || (info.globalInfo && info.globalInfo['country']!='中国')" >
                  <el-tooltip class="item" effect="dark" :content="info.globalInfo?info.globalInfo['ip']:'Loading...'" placement="top">
                      <div @click="copy(info.globalInfo?info.globalInfo['ip']:'')">
                          <el-tag style="width: 50px;" class="ml-2" type="success">{{ info.globalLay?info.globalLay+"ms":"-ms" }}</el-tag>
                          <el-text style="cursor: pointer;margin-left: 5px;white-space:nowrap;vertical-align: -3px;" class="font-background">{{ info.globalInfo?info.globalInfo['country']:"" }}</el-text>
                          <el-text style="cursor: pointer;margin-left: 5px;white-space:nowrap;vertical-align: -3px;" class="font-background">{{ info.globalInfo?info.globalInfo['isp']:"" }}</el-text>
                      </div>
                  </el-tooltip>
              </div>
          </transition>
        </div>
    </div>
</template>
  
<script lang="ts" setup>
const props = defineProps({
    isVisible: Boolean,
    IPinfo: Object
})
import CountryCode from "../assets/CountryCode.json"
import { reactive } from 'vue'
import { ElMessage } from 'element-plus'
import { toClipboard } from '@soerenmartius/vue3-clipboard'
const info=reactive({
    localInfo:null,
    globalInfo:null,
    localLay:0,
    globalLay:0,
})

const copy=(ip:string)=>{
    toClipboard(ip)
    ElMessage.success({
        dangerouslyUseHTMLString: true,
        message: `已经复制IP地址：<br><strong>${ip}</strong>`,
    })
}

const provinceMatch=(str:string)=>{
    const ChinaMainland=['内蒙古','黑龙江','河北','山西','吉林','辽宁','江苏','浙江','安徽','福建','江西','山东','河南','湖北','湖南','广东','海南','四川','贵州','云南','陕西','甘肃','青海','广西','西藏','宁夏','新疆','北京','天津','上海','重庆']
    for(let i in ChinaMainland)if(str.includes(ChinaMainland[i]))return ChinaMainland[i]
    return null
}

async function getLocalIp() {
    if(props.isVisible){
        try {
            const response = await  fetch('https://pubstatic.b0.upaiyun.com/?_upnode', { referrerPolicy: 'no-referrer' });
            let resp = await response.json();
            let localInfo:any={
                ip:resp['remote_addr'],
                isp:resp['remote_addr_location']['isp'],
                isChinaMainland:provinceMatch(resp['remote_addr_location']['province'])?true:false,
                province:provinceMatch(resp['remote_addr_location']['province']),
                city:resp['remote_addr_location']['city'].replace(/市$/, ""),
                area:''
            }
            info['localInfo']=localInfo
            if(props.IPinfo){
                props.IPinfo['localInfo']=localInfo
            }
        } catch (error) {
            info['localInfo']=null
        }
    }
    setTimeout(getLocalIp,info['localInfo']?5000:1000)
}
async function getGlobalIp() {
    if(props.isVisible){
        try {
            const response = await  fetch('https://api-ipv4.ip.sb/geoip', { referrerPolicy: 'no-referrer' });
            let resp = await response.json();
            let globalInfo:any={
                ip:resp['ip'],
                isp:resp['isp'],
                country:CountryCode[resp['country_code'] as keyof typeof CountryCode],
            }
            info['globalInfo']=globalInfo
            if(props.IPinfo){
                props.IPinfo['globalInfo']=globalInfo
            }
        } catch (error) {
            info['globalInfo']=null
        }
    }
    setTimeout(getGlobalIp,  info['globalInfo']?5000:1000)
}
getLocalIp()
getGlobalIp()
async function get_lay(url:string,type:'localLay'|'globalLay') {
    if(props.isVisible){
        try {
            var start_timestamp = new Date().getTime();
            await fetch(url,  { method: "HEAD", cache: "no-store", mode: 'no-cors', referrerPolicy: 'no-referrer' });
            info[type] = new Date().getTime() - start_timestamp;
        } catch (error) {
            info[type]=0
        }
    }
    setTimeout(get_lay, 1000,url,type)
}
get_lay('https://connectivitycheck.platform.hicloud.com/generate_204','localLay')
get_lay('https://cp.cloudflare.com/','globalLay')
</script>
  
<style>
.font-background{
  color: #344357;
  font-size: 14px;
}
.card{
    max-width: 800px;
    height:fit-content;
    display: block;
    margin:0 auto;
    background-color:#ffffff;
    padding:2%
}
@media (prefers-color-scheme: dark) {
    .card {
        background-color:rgb(18,18,18);
    }
    .font-background{
        color: rgb(193,206,230);
    }
}
</style>