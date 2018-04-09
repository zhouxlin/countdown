<template>
  <div>
    <section id="countdown">
     <h1>{{ msg }}</h1>
    </section>
    <section id="option">
     <el-button id="hold" type="primary" v-on:click="begin()">开始</el-button>
     <el-button id="end" type="primary" v-on:click="reset()">重置</el-button>
    </section>
    <section>
      <el-form :inline="true" :model="time" :rules="rule" ref="time" class="demo-form-inline">
        <el-form-item label="倒数几分钟啊" prop="minute">
         <el-input v-model="time.minute" placeholder="几分钟呢"></el-input>
        </el-form-item>
        <el-form-item>
        <el-button type="primary" v-on:click="confirm('time')">确定</el-button>
        </el-form-item>
      </el-form>
    </section>
  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
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
      intervalId: null,
      msg: '00:00:00',
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
    getTime (secend) {
      var time = {
        hours: '',
        minute: '',
        secend: ''
      }

      var hours = String(secend / 3600)
      time.hours = (hours.split('.')[0])
      secend = secend % 3600
      var minute = String(secend / 60)
      time.minute = (minute.split('.')[0])
      time.secend = String(secend % 60)
      if (time.secend.length === 1) {
        time.secend = '0' + time.secend
      }
      if (time.hours.length === 1) {
        time.hours = '0' + time.hours
      }
      if (time.minute.length === 1) {
        time.minute = '0' + time.minute
      }
      return time
    },
    // 输出时间
    printTime () {
      var time = this.getTime(this.secend)
      this.msg = time.hours + ':' + time.minute + ':' + time.secend
    },
    // 倒计时
    countDown () {
      this.printTime()
      if (this.secend <= 0) {
        window.clearInterval(this.intervalId)
      }
      this.secend -= 1
    },
    begin () {
      this.intervalId = setInterval(() => {
        this.countDown()
      }, 1000)
    },
    reset () {
      clearInterval(this.intervalId)
      this.setSecend()
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
#countdown {
  margin-top: 250px;
}
#option {
  margin-bottom: 50px;
}
h1 {
  font-size: 160px;
}
</style>
