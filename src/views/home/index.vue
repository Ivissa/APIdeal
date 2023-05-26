<template>
  <div style="padding: 40px;padding-left: 60px;">
    <h1>概述</h1>
    <div style="height: 10px;"></div>
    <el-divider></el-divider>
    <h2>Web服务API简介</h2>
    <p>高德Web服务API向开发者提供HTTP接口，开发者可通过这些接口使用各类型的地理数据服务，返回结果支持JSON和XML格式。</p>
    <p>Web服务API对所有用户开放。使用本组服务之前，需要申请应用Key。不同类型用户可获取不同的数据访问能力。</p>
    <el-image src="https://a.amap.com/lbs/static/img/summary_web_banner.png"></el-image>
    <el-divider></el-divider>
    <h2>功能介绍及体验</h2>
    <p>通过下面的demo，可以直观的体验我们提供的服务，点击下拉框，可切换需要体验的服务。</p>
    <el-select v-model="select.topic" placeholder="" @change="changeSelect">
      <el-option v-for="item in select" :key="item.topic" :value="item.topic"></el-option>
    </el-select>
    <el-table :data="nowSelect" border stripe style="margin-top: 20px;">
      <el-table-column prop="parameter" label="参数" min-width="20%"></el-table-column>
      <el-table-column prop="value" label="值" min-width="20%"></el-table-column>
      <el-table-column prop="remark" label="备注" min-width="50%"></el-table-column>
      <el-table-column prop="required" label="必选" min-width="10%"></el-table-column>
    </el-table>
    <el-button type="primary" style="margin-top: 20px;">运行</el-button>
  </div>
</template>
<script>

export default {
  data() {
    return {
      select: [
        {
          topic: 'IP定位',
          table: [
            {
              parameter: 'ip',
              value: '114.247.50.2',
              remark: '需要搜索的IP地址（仅支持国内）若用户不填写IP，则取客户HTTP之中的请求来进行定位',
              required: '否'
            }
          ]
        },
        {
          topic: '天气查询',
          table: [
            {
              parameter: 'city',
              value: '110101',
              remark: '需要查询天气的城市编码。',
              required: '否'
            }
          ]
        },
        {
          topic: '地理编码',
          table: [
            {
              parameter: 'address',
              value: '方恒国际中心A座',
              remark: '填写结构化地址信息:省+市+区+街道+门牌号',
              required: '是'
            },
            {
              parameter: 'city',
              value: '北京',
              remark: '查询城市，可选：城市中文、中文全拼、citycode、adcode',
              required: '否'
            }
          ]
        }
      ],
      nowSelect: []
    }
  },
  methods: {
    changeSelect(val) {
      const index = this.select.findIndex((item, index) => {
        return item.topic === val
      })
      this.nowSelect = this.select[index].table
      console.log(index)
    }
  }
}
</script>
<style lang="scss" scoped>

</style>
