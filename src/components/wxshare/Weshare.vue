<template>
  <div class="wx-share" id="share">
  </div>
</template>
<script>
import AppWeixinShareUtil from './We-Share'
export default {
  name: 'wx-share',
  data () {
    return {
      shareData: {
        title: '分享标题',
        desc: '分享描述',
        link: '分享图片',
        imgUrl: ''
      }
    }
  },
  methods: {
    async getWXShare (sharedata) {
      const url = 'http://api.huandengpai.com/api/weixin/js/config'
      const param = {
        title: sharedata.title,
        desc: sharedata.desc,
        link: sharedata.link,
        imgUrl: sharedata.imgUrl,
        visiting_url: window.location.href,
        weixin: 1
      }
      const response = await this.axios.post(url, param)
      let config = response.data.data
      config.jsApiList = ['onMenuShareTimeline', 'onMenuShareAppMessage', 'onMenuShareQQ', 'onMenuShareWeibo']
      config.debug = false
      let shareP = param
      AppWeixinShareUtil.setupWeixinShare(config, shareP)
    },
    setWxShare (sharedata, testAction) {
      console.log(testAction)
      AppWeixinShareUtil.setupWeixinShareOnly(sharedata)
    }
  },
  watch: {
    shareData () {
      // this.setWxShare(this.shareData, 'watch 监测share发生变化  设置分享地址')
    }
  },
  mounted () {
    this.getWXShare(this.shareData)
  }
}
</script>
