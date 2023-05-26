<template>
  <div class="page">
    <div style="display: flex;justify-content: space-between;width: 680px;">
      <h2>配额管理</h2>
      <p style="margin-top: 28px;">您可以在这里查看Web服务Key的每日调用量，还可以申请更高的调用次数。</p>
    </div>
    <!-- 配额管理表格 -->
    <div style="padding: 20px;background: white;">
      <el-table v-loading="loading" :data="select" border stripe style="margin-bottom: 20px;">
        <el-table-column prop="service" label="服务" min-width="15%"></el-table-column>
        <el-table-column prop="callamount" label="今日调用量" min-width="30%">
          <template #default="scope">
            <div style="display: flex;">
              <div style="width: 70%;padding-top: 4px;">
                <el-progress :percentage="Math.floor(scope.row.callamount/scope.row.callupper*100)" :format="(val)=>setText(val, scope.row.callamount)"></el-progress>
              </div>
              已用<div style="width: 20%;color: #1E90FF;">{{ Math.floor(scope.row.callamount/scope.row.callupper*100) + '%' }}</div>
            </div>
          </template>
        </el-table-column>
        <el-table-column prop="callupper" label="调用量上限（次/日）" min-width="15%"></el-table-column>
        <el-table-column prop="conupper" label="并发量上限（次/秒）" min-width="15%"></el-table-column>
        <el-table-column prop="state" label="状态" min-width="10%"></el-table-column>
        <el-table-column label="操作" min-width="15%">
          <template #default="scope">
            <a style="color: #1E90FF;" @click="upquota(scope.$index)">提升配额</a>
          </template>
        </el-table-column>
      </el-table>
      <el-progress :percentage="50" :format="(val)=>format(val, 20)"></el-progress>
      <!-- <button :class="{'s_type':active1, 'select':active2}" @click="change()">全部</button>
      <button class="s_type">2020年夏季</button> -->
    </div>
    <!-- 档位选择弹窗 -->
    <el-dialog :visible.sync="dialogMerge" title="提升配额" class="dialogClass">
      <div style="display: flex;">
        <div class="s_type" :class="{'select':active[0]}" @click="change(0)">
          <el-card class="box-card" :class="{'border': active[0]}">
            <div style="display: flex;color: red;margin: 30px 0 0 20px;">
              <div style="font-size: 35px;">￥300</div>
              <div style="font-weight: 15px;padding-top: 20px;">/10万次</div>
              <span class="status correct"></span>
            </div>
          </el-card>
        </div>
        <div class="s_type" :class="{'select':active[1]}" @click="change(1)">
          <el-card class="box-card" style="margin-left: 20px;" :class="{'border': active[1]}">
            <div style="display: flex;color: red;margin: 30px 0 0 20px;">
              <div style="font-size: 35px;">￥810</div>
              <div style="font-weight: 15px;padding-top: 20px;">/30万次</div>
            </div>
          </el-card>
        </div>
        <div class="s_type" :class="{'select':active[2]}" @click="change(2)">
          <el-card class="box-card" style="margin-left: 20px;" :class="{'border': active[2]}">
            <div style="display: flex;color: red;margin: 30px 0 0 5px;">
              <div style="font-size: 35px;">￥2400</div>
              <div style="font-weight: 15px;padding-top: 20px;">/100万次</div>
            </div>
          </el-card>
        </div>
        <div class="s_type" :class="{'select':active[3]}" @click="change(3)">
          <el-card class="box-card" style="margin-left: 20px;" :class="{'border': active[3]}">
            <div style="display: flex;color: red;margin: 30px 0 0 -10px;">
              <div style="font-size: 35px;">￥10500</div>
              <div style="font-weight: 15px;padding-top: 20px;">/500万次</div>
            </div>
          </el-card>
        </div>
      </div>
      <div style="display: flex;padding-top: 15px;">
        <div class="s_type" :class="{'select':active[4]}" @click="change(4)">
          <el-card class="box-card" :class="{'border': active[4]}">
            <div style="display: flex;color: red;margin: 30px 0 0 -15px;">
              <div style="font-size: 35px;">￥18000</div>
              <div style="font-weight: 15px;padding-top: 20px;">/1000万次</div>
            </div>
          </el-card>
        </div>
        <div class="s_type" :class="{'select':active[5]}" @click="change(5)">
          <el-card class="box-card" style="margin-left: 20px;" :class="{'border': active[5]}">
            <div style="display: flex;color: red;margin: 30px 0 0 -15px;">
              <div style="font-size: 35px;">￥60000</div>
              <div style="font-weight: 15px;padding-top: 20px;">/5000万次</div>
            </div>
          </el-card>
        </div>
        <el-card class="box-card" style="margin-left: 20px;">
          <p style="color:gray;padding: 30px 0 0 25px;font-size: 15px;">自定义流量包大小</p>
        </el-card>
      </div>
      <div style="text-align: center;padding-top: 40px;">
        <el-button type="primary" @click="purchase">购买</el-button>
      </div>
    </el-dialog>
    <div class="pay">
      <el-dialog :visible.sync="dialogPay" title="付款" style="text-align: center;" custom-class="customWidth">
        <img src="../../assets/quota/payment.png" style="width: 300px;">
      </el-dialog>
    </div>
  </div>
</template>
<script>
export default {
  data() {
    return {
      active: [],
      select: [
        {
          service: '地理编码',
          callamount: 400,
          callupper: 5000,
          conupper: 30,
          state: '正常'
        },
        {
          service: '逆地理编码',
          callamount: 800,
          callupper: 5000,
          conupper: 30,
          state: '正常'
        },
        {
          service: '搜索服务-关键字查询',
          callamount: 5000,
          callupper: 5000,
          conupper: 30,
          state: '异常'
        }
      ],
      // 提升配额弹窗
      dialogMerge: false,
      // 付款弹窗
      dialogPay: false,
      quotaIndex: ''
    }
  },
  mounted: function() {
    this.active = Array(6).fill(false)
  },
  methods: {
    setText(val, callamount) {
      return callamount
    },
    upquota(index) {
      this.dialogMerge = true
      console.log(index)
    },
    change(index) {
      if (this.active[index]) {
        this.active = Array(6).fill(false)
      } else {
        this.active = Array(6).fill(false)
        this.active[index] = !this.active[index]
      }
      // 所选择的档位下标
      this.quotaIndex = this.active.indexOf(true)
      console.log(this.active.indexOf(true))
    },
    purchase() {
      this.dialogPay = true
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
.dialogClass .el-dialog__body {
  width: 1000px;
  margin-left: 20px;
  padding-bottom: 40px;
}
.box-card{
  width: 225px;
  height: 150px;

}
.s_type {
  border: none;
  border-radius: 5px;
  background-color: #f3f3f3;
  color: #606266;
  position: relative;
}
.select {
  background-color: #ebf3ff;
  color: #5999fc;
}

.select:before {
  content: '';
  position: absolute;
  right: 0;
  bottom: 0;
  border: 12px solid #5999fc;
  border-top-color: transparent;
  border-left-color: transparent;
}
.select:after {
  content: '';
  width: 5px;
  height: 10px;
  position: absolute;
  right: 4px;
  bottom: 5px;
  border: 2px solid #fff;
  border-top-color: transparent;
  border-left-color: transparent;
  transform: rotate(45deg);
}
.border {
  border: 2px solid #1E90FF;
}
.pay {
  width: 200px;
  height: 200px;
}
.customWidth{
    width:20%;
}
</style>
