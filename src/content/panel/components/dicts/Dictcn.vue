<template>
<section>
  <div class="dict-dictcn" v-if="result">
    <div class="chart" v-if="result.chart">
      <highcharts :options="chartOpt"></highcharts>
    </div>
    <ul class="etym" v-if="result.etym">
      <li class="etym-item" v-for="etym in result.etym">{{ etym }}</li>
    </ul>
  </div>
</section>
</template>

<script>
export default {
  name: 'Dictcn',
  props: ['result'],
  computed: {
    chartOpt () {
      let chartData = this.result.chart
      if (!chartData) { return }

      return {
        chart: {
          spacingTop: 10,
          spacingRight: 0,
          spacingBottom: 20,
          spacingLeft: 0,
          width: 270,
          height: 200,
          style: {
            fontFamily: '"Helvetica Neue", Helvetica, Arial, "Hiragino Sans GB", "Hiragino Sans GB W3", "Microsoft YaHei UI", "Microsoft YaHei", "WenQuanYi Micro Hei", sans-serif'
          }
        },
        title: {
          text: this.result.title + ' 释义常用度分布图',
          align: 'center',
          verticalAlign: 'bottom',
          margin: 20,
          style: {
            color: '#808080',
            fontSize: '12px'
          }
        },
        tooltip: {
          formatter () {
            return '常用度:' + this.percentage + ' %'
          },
          style: {
            padding: '4px',
            lineHeight: '20px'
          }
        },
        plotOptions: {
          pie: {
            allowPointSelect: true,
            cursor: 'pointer',
            startAngle: 90,
            size: 100,
            slicedOffset: 8,
            dataLabels: {
              distance: 14,
              softConnector: false,
              useHTML: true,
              connectorPadding: 6,
              connectorColor: '#808080',
              formatter () {
                return this.point.name
              },
              style: {
                color: '#666',
                fontSize: '12px'
              }
            }
          }
        },
        credits: {
          enabled: false
        },
        series: [{
          type: 'pie',
          name: '分布比例：',
          colors: ['#19B29F', '#53C8BA', '#62DDCF', '#82E3D9', '#9EF0E8', '#74E7DA', '#93F3E8', '#ACFBF2', '#BFF3EE', '#D0FFFB'],
          data: Object.keys(chartData).map(i => [`<font title="${chartData[i].sense}">${chartData[i].sense}</font>`, chartData[i].percent])
        }]
      }
    }
  }
}
</script>

<style scoped>
.dict-dictcn {
  padding: 10px;
}

.chart {
  display: flex;
  justify-content: center;
}

.etym {
  margin: 0;
  padding: 0;
}

.etym-item {
  list-style: none;
}
</style>
