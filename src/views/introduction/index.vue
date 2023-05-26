<template>
  <div style="padding: 40px;padding-left: 60px;">
    <h1>入门指南</h1>
    <p>本指南是向 webAPI添加地图的快速入门指南。</p>
    <el-divider></el-divider>
    <h2>第 1 步：申请”Web服务API”密钥（key）</h2>
    <a style="color:blue;text-decoration: underline ;">点我获取Key>></a>
    <div style="height: 15px;"></div>
    <el-divider></el-divider>
    <h2>第 2 步：拼接HTTP请求URL</h2>
    <p>第一步申请的key需作为必填参数一同发送</p>
    <div style="height: 15px;"></div>
    <el-divider></el-divider>
    <h2>第 3 步：接收HTTP请求返回的数据（JSON或XML格式），解析数据。</h2>
    <p>如无特殊声明，接口的输入参数和输出数据编码全部统一为UTF-8。</p>
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
