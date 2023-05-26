<template>
  <div class="page">
    <div style="display: flex;justify-content: space-between;width: 680px;">
      <h2>流量详情</h2>
    </div>
    <div style="padding: 20px;background: white;">
      <div style="display: flex;">
        <p>时间区间</p>
        <el-radio-group v-model="timeSection" style="padding: 20px 0 0 55px;" @change="radio">
          <el-radio v-model="timeSection" label="hour">最近一小时</el-radio>
          <el-radio v-model="timeSection" label="day">最近一天</el-radio>
          <el-radio v-model="timeSection" label="week">最近一周</el-radio>
        </el-radio-group>
      </div>
      <div style="display: flex;">
        <p>横坐标精度</p>
        <el-radio-group v-model="ruleForm.desc" style="padding: 20px 0 0 40px;">
          <el-radio label="按分钟显示"></el-radio>
          <el-radio label="按小时显示"></el-radio>
          <el-radio label="按天显示"></el-radio>
        </el-radio-group>
      </div>
    </div>
    <div class="dashboard-container">
      <adminDashboard :time-section="timeSection"></adminDashboard>
      <!-- <component :is="currentRole" /> -->
    </div>
  </div>
</template>
<script>
import adminDashboard from './admin'
export default {
  components: {
    adminDashboard
  },
  data() {
    return {
      currentRole: 'adminDashboard',
      ruleForm: {
        name: '',
        region: '',
        date1: '',
        date2: '',
        delivery: false,
        type: [],
        resource: '',
        desc: ''
      },
      timeSection: 'hour'
    }
  },
  methods: {
    radio(value) {
      console.log(value)
      console.log(this.timeSection)
    }
  }
}
</script>
<style lang="scss">
.page {
  padding: 40px;
  padding-left: 60px;
  min-height: 100vh;
  background-color: rgb(250, 250, 250);
}
</style>
