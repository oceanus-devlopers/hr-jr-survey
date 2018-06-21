<template>
  <div class="wx-share" id="share" style="height:0rem;overflow:hidden;">
    <img class='share-img' src="@/assets/images/share.png" alt="" style="visibility: hidden;">
  </div>
</template>
<script>
import AppWeixinShareUtil from './We-Share'
export default {
  name: 'wx-share',
  data () {
    return {
      shareData: {
        title: '全掌柜平台问卷调研',
        desc: '让每个家庭拥有自己的保险规划师',
        link: '',
        imgUrl: ''
      }
    }
  },
  methods: {
    async getWXShare (sharedata) {
      const url = 'http://api.huandengpai.com/api/weixin/js/config'
      const shareImg = document.getElementsByClassName('share-img')[0].src
      const shareUrl = window.location.origin + '/goto.html?goto=' + encodeURIComponent('https://mp.weixin.qq.com/s/cRCDMTCmARDJ3MTMDEciCg')
      const param = {
        title: sharedata.title,
        desc: sharedata.desc,
        link: shareUrl,
        imgUrl: shareImg,
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
      AppWeixinShareUtil.setupWeixinShareOnly(sharedata)
    }
  },
  mounted () {
    this.getWXShare(this.shareData)
  }
}
</script>
