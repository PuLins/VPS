<!-- 数据图表-总数统计 -->
<template>
<!-- v-loading="loading" -->
    <section  id="riskBox">
      <!-- 四边小角 -->
      <div class="left-top"></div>
      <div class="right-top"></div>
      <div class="let-bottom"></div>
      <div class="right-bottom"></div>
      <div class="header">
          <span>总数统计</span>
     </div>
      <div id="pieChart"></div>
    </section>
</template>
<script>
export default {
  name: '',
  data () {
    return {
    }
  },
  computed: {
    loading () {
      return this.$store.getters.loadingObj.ECRisk
    },
    timeState () {
      return this.$store.getters.timeState;
    }
  },
  mounted () {
    let _this = this;
    _this.$nextTick(function () {
      _this.drawLine(1);
    })
  },
  methods: {
    drawLine (timeState) {
      let chartData = {};
      switch (+timeState) {
        // 年
        case 3: {
          chartData.drivingOrder = this.$store.getters.count.driveAll;// 总行车时长
          chartData.allKilometre = this.$store.getters.count.mileAll;// 总公里数
          chartData.allOnLine = this.$store.getters.count.onLineAll;// 总在线次数
          chartData.allPilace = this.$store.getters.count.alarmAll;// 总报警次数;
          this.chartInit(chartData);
          break;
        }
        // 月
        case 2: {
          chartData.drivingOrder = this.$store.getters.count.driveAll;// 总行车时长
          chartData.allKilometre = this.$store.getters.count.mileAll;// 总公里数
          chartData.allOnLine = this.$store.getters.count.onLineAll;// 总在线次数
          chartData.allPilace = this.$store.getters.count.alarmAll;// 总报警次数;
          this.chartInit(chartData);
          break;
        }
        // 日
        case 1: {
          chartData.drivingOrder = this.$store.getters.count.driveAll;// 总行车时长
          chartData.allKilometre = this.$store.getters.count.mileAll;// 总公里数
          chartData.allOnLine = this.$store.getters.count.onLineAll;// 总在线次数
          chartData.allPilace = this.$store.getters.count.alarmAll;// 总报警次数;
          this.chartInit(chartData);
          break;
        }
      }
    },
    chartInit (chartData) {
      // 基于准备好的dom，初始化echarts实例
      let chartDiv = document.querySelector('#pieChart');
      // 自适应宽高
      let myChartContainer = function () {
        chartDiv.style.width = $('#riskBox').width() + 'px';
        chartDiv.style.height = $('#riskBox').height() + 'px';
      };
      myChartContainer();
      let riskChart = this.$echarts.init(chartDiv);
      riskChart.setOption({
        tooltip: {
          trigger: 'item',
          position: ['10%', '70%'],
          formatter: '{b} : <br/> {c} ({d}%)'
        },
        legend: {
          orient: 'vertical',
          data: ['总行车时长(分钟)', '总公里数(km)', '总在线次数(次)', '总报警次数(次)'],
          textStyle: {
            color: '#fff'
          }
        },
        series: [
          {
            name: '总数统计',
            type: 'pie',
            radius: ['60%', '80%'],
            avoidLabelOverlap: false,
            label: {
              normal: {
                show: false,
                position: 'center'
              },
              emphasis: {
                show: true,
                textStyle: {
                  fontSize: '18',
                  fontWeight: 'bold'
                }
              }
            },
            labelLine: {
              normal: {
                show: false
              }
            },
            data: [
              {value: chartData.drivingOrder, name: '总行车时长(分钟)'},
              {value: chartData.allKilometre, name: '总公里数(km)'},
              {value: chartData.allOnLine, name: '总在线次数(次)'},
              {value: chartData.allPilace, name: '总报警次数(次)'}
            ]
          }
        ]
      });
      // 浏览器大小改变时重置大小
      window.addEventListener('resize', function () {
        myChartContainer();
        riskChart.resize();
      });
    }
  },
  watch: {
    timeState: function (newVal, oldVal) {
      this.drawLine(newVal);
    }
  }
}
</script>


