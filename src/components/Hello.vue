<template>
  <div class="hello">
    <P>
      哥是图灵机器棱，请您回答：
      <input v-model="question"/>
    </P>
    <p>哥的回答：{{ answer }}</p>
  </div>
</template>

<script>
import _ from 'lodash'
export default {
  data () {
    return {
      question: '',
      answer: '只有你提了问题， 才会出现答案！'
    }
  },
  watch: {
    question: function (newQuestion) {
      this.answer = '等待你停止输入。。。'
      this.getAnswer()
    }
  },
  methods: {
    // _.debounce是一个通过 lodash 限制操作频率的函数。
    // 限制yesno.wtf/api的频率，ajax请求直到用户输入完毕彩绘发出
    getAnswer: _.debounce(
      function () {
        var vm = this
        if (this.question.indexOf('？') === -1) {
          vm.answer = '问题的结尾通常都是问号的哦。^_^'
          return
        }
        vm.answer = '努力思考中。。。'
        let baseUrl = 'api/robot/index?'
        let info = encodeURI(this.question)
        let userId = '13760795885'
        let key = 'e7168d4ec8a09bf383c9395a0ca2f42e'
        let targetUrl = baseUrl + 'info=' + info + '&userid=' +
                        userId + '&key=' + key
        this.$http.get(targetUrl)
          .then((response) => {
            console.log(response)
            vm.answer = response.data.result.text
          })
          .catch(function (error) {
            vm.answer = '出错了，无法访问图灵机器棱api。' + error
          })
      },
      500
      )
  }
}
</script>
