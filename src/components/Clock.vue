<template>
  <div>
    <h1>{{ clock }}</h1>
  </div>
</template>

<script>
export default {
  name: 'clock',
  props: {
  },
  data () {
    return {
      clock: '00:00:00'
    }
  },
  methods: {
    show (secend) {
      var time = {
        hours: '',
        minute: '',
        secend: ''
      }
      console.log(secend)
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
      this.clock = time.hours + ':' + time.minute + ':' + time.secend
    }
  }
}
</script>

<style scoped>
h1 {
  font-size: 160px;
}
</style>
