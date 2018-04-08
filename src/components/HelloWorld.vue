<template>
  <div>
    <section id="countdown">
     <h1>{{ msg }}</h1>
    </section>
    <section id="option">
     <el-button id="hold" type="primary">暂停</el-button>
     <el-button id="end" type="primary">结束</el-button>
    </section>
    <section>
      <el-form :inline="true" class="demo-form-inline">
        <el-form-item label="审批人">
         <el-input placeholder="审批人"></el-input>
        </el-form-item>
        <el-form-item>
        <el-button type="primary">开始计时</el-button>
        </el-form-item>
      </el-form>
    </section>
  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  data () {
    return {
      msg: '20:23:32',
      time: 0
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
#countdown {
  margin-top: 200px;
}
#option {
  margin-bottom: 20px;
}
h1 {
  font-size: 100px;
}
</style>
