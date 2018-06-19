<template>
  <div id="app">
    <Wxshare></Wxshare>
    <div class="contain">
      <div class="page-survey">
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
        <div class="item">
          <input type="text" id="name" placeholder="姓名">
        </div>
        <div class="item">
          <div>
            <input type="radio" id="Male">
            <label for="">男</label>
          </div>
          <div>
            <input type="radio" id="Female">
            <label for="">女</label>
          </div>
        </div>
        <div id="submit" @click='submit'>提交</div>
      </div>
    </div>
  </div>
</template>
<script>
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
      ]
    }
  },
  methods: {
    submit () {
      const formData = this.formData
      for (let i = 0; i < formData.length; i++) {
        if (formData[i].data.length === 0) {
          console.log(formData[i].id)
        }
      }
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
