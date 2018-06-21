<template>
  <div id="app">
    <Wxshare></Wxshare>
    <div class="loading" v-if='isLoading'>
      <img style="position: absolute;width: 30%;height: auto;left: 35%;top: 4rem;" src="@/assets/images/_loadingC.png" alt="">
      <img class="_loadingZ" style="position: absolute;width: 30%;height: auto;left: 35%;top: 4rem;" src="@/assets/images/_loadingZ.png" alt="">
    </div>
    <div class="contain">
      <div class="page-survey">
        <img src="@/assets/images/wenbg.jpg" alt="" class="bg-img">
        <div class="sur-c">
          <div class="item" :key='item.id' v-for='(item, num) in formData'>
            <div class="topic">{{item.topic}}</div>
            <div class="options-box">
              <div class="options" :key='index' v-for='(option, index) in item.options'>
                <label :for="'s'+num+index">
                  {{option}}
                </label>
                <input :type="item.type" :id="'s'+num+index" v-model="item.data" v-bind:value="option">
              </div>
            </div>
          </div>
          <div class="con-users-news">
            <div class="user-text">为更好的提供保障服务，请填写以下信息，感谢!</div>
            <div class="user-list-box">
              <div class='lb'>
                <div class="user-list1">
                  <span class='fields'>年龄</span>
                  <div>
                    <input type="number" id="age" placeholder="" v-model="age">
                  </div>
                </div>
                <div class="user-list2">
                  <span class='fields'>姓名</span>
                  <div>
                  <input type="text" id="name" placeholder="" v-model="name">
                  </div>
                </div>
              </div>
              <div class='lb'>
                <div class="user-list1">
                  <span class='fields'>性别</span>
                  <div class="sex-list">
                      <div>
                        <label for="">男</label>
                        <input type="radio" id="Male" v-model="Sex" value="男">
                      </div>
                      <div>
                        <label for="">女</label>
                        <input type="radio" id="Female" v-model="Sex" value="女">
                      </div>
                  </div>
                </div>
                <div class="user-list2">
                  <span class='fields'>电话</span>
                  <div>
                  <input type="number" id="phone" placeholder="" v-model="phone">
                  </div>
                </div>
              </div>
            </div>
          </div>
          <div id="submit" @click='submit'>提交</div>
        </div>
      </div>
    </div>
    <div class="pops-suc" v-if='pop'>
      <div class="pops-inner">
        <img src="@/assets/images/success.png" alt="">
      </div>
    </div>
  </div>
</template>
<script>
import axios from 'axios'
import Wxshare from '@/components/wxshare/Weshare'
export default {
  name: 'App',
  components: {
    Wxshare
  },
  data () {
    return {
      formData: [
        {
          topic: '1、为自己的安全添加保障是对家人的一种关爱吗？',
          options: ['是', '不是'],
          id: 1,
          data: [],
          type: 'radio'
        },
        {
          topic: '2、您每年给自己和家人投入保障（保险）预算是多少？',
          options: ['0-1000', '1000-5000', '5000-10000', '10000-30000', '30000以上'],
          id: 2,
          data: [],
          type: 'radio'
        },
        {
          topic: '3、您或您的家人目前已拥有以下哪种保障?  （可多选）',
          options: ['子女教育保障', '人身安全保障', '住院医疗保障', '重大疾病保障', '投资理财保障（如分红、投资类）', '家庭收入保障', '退休养老保障'],
          id: 3,
          data: [],
          type: 'checkbox'
        },
        {
          topic: '4、您对以下哪些保险感兴趣？（可多选）',
          options: ['子女教育保障', '重大疾病', '意外伤害', '住院医疗', '退休养老', '投资理财'],
          id: 4,
          data: [],
          type: 'checkbox'
        },
        {
          topic: '5、您会用什么方式了解保险产品、知识？',
          options: ['网络搜索', '重大疾病', '意外伤害', '住院医疗', '退休养老', '投资理财'],
          id: 5,
          data: [],
          type: 'radio'
        }
      ],
      postData: {},
      name: '',
      Sex: '',
      isSubmit: true,
      phone: '',
      age: '',
      openid: '',
      flag: true,
      isLoading: true,
      localStorageKey: 'aGFpZXItZmluYW5jZS1zdXJ2ZXJ5LW9wZW5pZA',
      pop: false
    }
  },
  methods: {
    submit () {
      this.isSubmit = true
      const formData = this.formData
      this.postData['openid'] = this.openid
      this.removeError()
      for (let i = 0; i < formData.length; i++) {
        if (formData[i].data.length === 0) {
          let divs = document.createElement('div')
          divs.innerHTML = '必填'
          divs.setAttribute('class', 'error')
          document.getElementsByClassName('topic')[i].appendChild(divs)
          let _top = document.getElementsByClassName('topic')[i].offsetTop
          document.getElementsByClassName('contain')[0].scrollTop = _top
          this.isSubmit = false
          // break
          return false
        } else {
          let d = 'choicequestion_' + (i + 1)
          let pd = formData[i].data
          if (this.type(pd) === 'Array') {
            pd = pd.join(';')
          }
          this.postData[d] = pd
        }
      }
      if (this.age.replace(/ /g, '').length === 0) {
        this.isSubmit = false
        this.error('年龄必填')
        return false
      } else {
        this.postData.age = this.age
      }
      if (this.name.replace(/ /g, '').length === 0) {
        this.isSubmit = false
        this.error('姓名必填')
        return false
      } else {
        this.postData.name = this.name
      }
      if (this.Sex.replace(/ /g, '').length === 0) {
        this.isSubmit = false
        this.error('性别必选')
        return false
      } else {
        this.postData.sex = this.Sex
      }
      if (this.phone.replace(/ /g, '').length === 0) {
        this.isSubmit = false
        this.error('手机号必填')
        return false
      } else {
        this.postData.phone = this.phone
      }
      let isMobile = /^(?:13\d|15\d|17\d|18\d)\d{5}(\d{3}|\*{3})$/
      if (!isMobile.test(this.phone)) {
        this.isSubmit = false
        this.error('   手机号格式不正确')
        return false
      } else {
        this.postData.phone = this.phone
      }
      if (this.isSubmit && this.flag) {
        this.sus()
      }
    },
    removeError () {
      let ddd = document.getElementsByClassName('error')
      for (let k = 0; k < ddd.length; k++) {
        let de = ddd[k]
        de.parentNode.removeChild(de)
      }
    },
    error (text) {
      let divs = document.createElement('div')
      divs.innerHTML = text
      divs.setAttribute('class', 'error')
      document.getElementsByClassName('user-text')[0].appendChild(divs)
    },
    sus () {
      this.flag = false
      let that = this
      axios.post('/ajaxapi/h5/haier/newyear/finance/survey/ajaxSaveData', this.postData).then(function (response) {
        if (response.data.status === 200) {
          that.flag = true
          that.pop = true
        } else if (response.data.status === 400) {
          alert('参数错误')
        } else {
          alert('网络异常')
        }
      })
    },
    type (obj) {
      return Object.prototype.toString.call(obj).slice(8, -1)
    },
    getQueryString: function (name) {
      let reg = new RegExp(name + '=([^&#]*)', 'i')
      let value = reg.exec(window.location.href)
      if (value == null) return null
      return decodeURI(value[1])
    },
    getLocalStorageOpenid () {
      let localStorageOpenid = localStorage.getItem(this.localStorageKey)
      if (!localStorageOpenid) {
        let localhref = window.location.href
        let pregMatchResult = new RegExp('__source_user_id' + '=([^&#]*)', 'i').exec(localhref)
        if (pregMatchResult !== null) {
          localStorage.setItem(this.localStorageKey, pregMatchResult[1])
          localStorageOpenid = pregMatchResult[1]
        } else {
          localhref = encodeURIComponent(localhref)
          let redirectUri = ''
          // 海尔微信授权服务
          let hostname = window.location.hostname
          if (hostname.indexOf('haierclub.') === 0) {
            redirectUri = encodeURIComponent('http://hicweixin.haier.net/hs/h/redirect/redirect_url.html?url=' + localhref)
            window.location.href = 'https://open.weixin.qq.com/connect/oauth2/authorize?appid=wx7d3377afc607dcae&redirect_uri=' + redirectUri + '&response_type=code&scope=snsapi_base&state=STATE#wechat_redirect'
          } else {
            redirectUri = encodeURIComponent('http://hicfuweixin.haier.net/hs/h/redirect/redirect_url.html?url=' + localhref)
            window.location.href = 'https://open.weixin.qq.com/connect/oauth2/authorize?appid=wx37a78a04145aafcd&redirect_uri=' + redirectUri + '&response_type=code&scope=snsapi_base&state=STATE#wechat_redirect'
          }
        }
      }
      return localStorageOpenid
    },
    async getIslottyey () {
      const urlKey = '/ajaxapi/h5/haier/newyear/finance/survey/ajaxIsExist?openid=' + this.openid
      await axios.get(urlKey).then((res) => {
        if (res.status === 200) {
          const data = res.data
          if (data.status === 200) {
            const rdata = data.data
            if (rdata) {
              // this.isLoading = false
              let host = window.location.hostname
              if (host === 'haierclub.oceanus-data.com') {
                window.location.href = window.location.origin + '/haier/user/game/show/entercopy?lottery_nid=888&openid=' + this.openid
              } else if (host === 'haierclub.huandengpai.com') {
                window.location.href = window.location.origin + '/haier/user/game/show/entercopy?lottery_nid=1024&openid=' + this.openid
              }
            } else {
              setTimeout(() => {
                this.isLoading = false
              }, 200)
            }
          } else if (data.status === 400) {
            alert('参数错误')
          }
        } else {
          alert('网络异常')
        }
      })
    }
  },
  created () {
    this.openid = this.getLocalStorageOpenid()
    if (this.openid) {
      this.getIslottyey()
    }
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
}
</style>
<style>
  @import url(./assets/css/reset.css);
  @import url(./assets/css/common.css);
</style>
