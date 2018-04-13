<template>
  <div>
    <section id="countdown">
      <clock ref="clock"></clock> <!-- 引用显示时钟组件 -->
    </section>
    <section id="option">
     <el-button id="hold" type="primary" v-on:click="begin()" :disabled="status.status == 3">{{ status.btn }}</el-button>
     <el-button id="end" type="primary" v-on:click="reset()" :disabled="status.status == 1">重置</el-button>
    </section>
    <section>
      <el-form :inline="true" :model="time" :rules="rule" ref="time">
        <el-form-item label="倒数几分钟啊" prop="minute">
         <el-input v-model="time.minute" placeholder="几分钟呢" @keyup.native="confirm('time')" :disabled="status.status != 0"></el-input>
        </el-form-item>
        <el-form-item>
        </el-form-item>
      </el-form>
    </section>
  </div>
</template>

<script>
import Clock from './Clock'

export default {
  components: {Clock}, // 引入时钟组件
  name: 'CountDown',
  data () {
    var checkMinute = (rule, value, callback) => {
      if (!value) {
        return callback(new Error('不能为空'))
      }
      setTimeout(() => {
        if (!Number.isInteger(Number(value))) {
          callback(new Error('请输入整数值'))
        } else {
          callback()
        }
      }, 1000)
    }
    return {
      status: {
        btn: '开始',
        btnTxt: ['开始', '暂停', '继续'],
        status: 0,
        statusArr: [0, 1, 2, 3] // 0: 初始，1：计时中，2：暂停，3:结束
      },
      intervalId: null,
      time: {
        minute: ''
      },
      secend: 0,
      rule: {
        minute: [
          {validator: checkMinute, trigger: 'blur'}
        ]
      }
    }
  },
  methods: {
    confirm (time) {
      this.$refs[time].validate((valid) => {
        if (valid) {
          this.setSecend()
          this.printTime()
        } else {
          console.log('error submit!!')
          return false
        }
      })
    },
    setSecend () {
      this.secend = (this.time.minute * 60)
    },
    // 输出时间
    printTime () {
      this.$refs.clock.show(this.secend)
    },
    // 倒计时
    countDown () {
      this.printTime()
      if (this.secend <= 0) {
        window.clearInterval(this.intervalId)
        this.status.btn = this.status.btnTxt[1]
        this.status.status = this.status.statusArr[3]
        this.setSecend() // 倒数结束，一开始的秒数
      }
      this.secend -= 1
    },
    begin () {
      if (this.status.status === 0 || this.status.status === 2) {
        if (this.secend === 0) {
          this.$message('设置个时间呗')
          return
        }
        this.status.btn = this.status.btnTxt[1]
        this.status.status = this.status.statusArr[1]
        this.intervalId = setInterval(() => {
          this.countDown()
        }, 1000)
      } else if (this.status.status === 1) {
        this.status.btn = this.status.btnTxt[2]
        this.status.status = this.status.statusArr[2]
        clearInterval(this.intervalId)
      }
    },
    reset () {
      clearInterval(this.intervalId)
      this.setSecend()
      this.printTime()
      this.status.btn = '开始'
      this.status.status = this.status.statusArr[0]
    }
  },
  mounted () {
    this.status.btn = this.status.btnTxt[0]
    this.status.status = this.status.statusArr[0]
  }
}

</script>

<style scoped>
#countdown {
  margin-top: 250px;
}
#option {
  margin-bottom: 50px;
}
</style>
