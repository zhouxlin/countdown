<template>
  <div>
    <h1 :class="{ red: isRed }">{{ clock }}</h1>
  </div>
</template>

<script>
export default {
  name: 'clock',
  props: {
  },
  data () {
    return {
      clock: '00:00:00',
      isRed: false
    }
  },
  methods: {
    show (secend) {
      this.changeColor(secend)
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
      // 处理输出的格式
      time.secend = '0' + time.secend
      time.secend = time.secend.substr(-2, 2)
      time.hours = '0' + time.hours
      time.hours = time.hours.substr(-2, 2)
      time.minute = '0' + time.minute
      time.minute = time.minute.substr(-2, 2)
      this.clock = time.hours + ':' + time.minute + ':' + time.secend
    },
    changeColor (secend) {
      if (secend < 10) {
        this.isRed = true
      } else {
        this.isRed = false
      }
    }
  }
}
</script>

<style scoped>
h1 {
  font-size: 160px;
}
.red {
  color: #C00D0D;
}
</style>
