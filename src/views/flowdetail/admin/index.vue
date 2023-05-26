<template>
  <div class="dashboard-editor-container">

    <el-row style="background:#fff;padding:16px 16px 0;margin-bottom:32px;">
      <line-chart :chart-data="lineChartData" :char-type="timeSection" />
    </el-row>

  </div>
</template>

<script>
import LineChart from './components/LineChart'

const lineChartData = {
  week: {
    quotaData: [180, 90, 160, 180, 200, 220, 180],
    actualData: [150, 82, 91, 154, 162, 140, 145]
  },
  hour: {
    quotaData: [150, 60, 50, 40, 80, 100, 90, 70, 120, 125, 130, 110],
    actualData: [130, 55, 42, 37, 46, 26, 40, 20, 114, 108, 107, 102]
  },
  day: {
    quotaData: [150, 60, 50, 40, 80, 100, 90, 70, 120, 125, 130, 110, 180, 90, 160, 180, 200, 220, 180, 120, 160, 50, 70, 80, 100],
    actualData: [130, 55, 42, 37, 46, 26, 40, 20, 114, 108, 107, 102, 150, 82, 91, 154, 162, 140, 145, 80, 70, 69, 38, 60, 87]
  }
}

export default {
  name: 'DashboardAdmin',
  components: {
    LineChart
  },
  props: {
    timeSection: {
      type: String,
      required: true
    }
  },
  data() {
    return {
      lineChartData: ''
    }
  },
  watch: {
    timeSection() {
      this.refresh()
    }
  },
  mounted() {
    this.refresh()
  },
  methods: {
    refresh() {
      if (this.timeSection === 'week') {
        this.lineChartData = lineChartData.week
      } else if (this.timeSection === 'hour') {
        this.lineChartData = lineChartData.hour
      } else {
        this.lineChartData = lineChartData.day
      }
    }
  }
}
</script>

<style lang="scss" scoped>
.dashboard-editor-container {
  padding: 32px;
  background-color: white;
  position: relative;

  .github-corner {
    position: absolute;
    top: 0px;
    border: 0;
    right: 0;
  }

  .chart-wrapper {
    background: #fff;
    padding: 16px 16px 0;
    margin-bottom: 32px;
  }
}

@media (max-width:1024px) {
  .chart-wrapper {
    padding: 8px;
  }
}
</style>
