<template lang="pug">
#analysis.bg-green.w-100
  b-row.ml-2
    b-col.vh-100.boxshadow(cols=5)
      div.d-flex.ml-4.mt-5
        p.text-analysis 專注度分析
      b-row
        b-col(cols='6')
          div.ml-4.mb-2
            span.title 今日
            span.data.ml-3 {{ nowdate | dateformat }}
          div.border.d-flex.ml-4
            div.text-center.w-50
              p.text-green.mb-0 9
              p.text-title 代辦事項
            div.text-center.w-50
              p.text-orange.mb-0 7
              p.text-title 已完成
        b-col(cols='6')
          div.mb-2
            span.title 本週
          div.border.d-flex
            div.text-center.w-50
              p.text-green.mb-0 54
              p.text-title 代辦事項
            div.text-center.w-50
              p.text-orange.mb-0 48
              p.text-title 已完成
      div.mt-5
        p.week.text-center
          img(:src='require("../assets/icon-arrow--prev.svg")')
          span.p-3 {{ nowdate | datformat }} ~ {{ nowdate | dateformat }}
          img(:src='require("../assets/icon-arrow--next.svg")')
        div
          apexchart(width="400" type="bar" :options='options' :series='series')
    b-col(cols='6').w-100
      b-row.text-center
        b-col(cols='12')
          div.text-right.mt-4
            p.dateText.mr-3 {{ nowdate | dateformat }}
            p.dateText {{ nowdate | timeformat }}
        b-col(cols=12)
          div.mt-3
            p.text-time-list {{ timeText }}
            div.d-flex.align-items-center.justify-content-center
              b-btn.circle(to='/settings' pill)
                img(:src='require("../assets/icon-bell.svg")')
              b-btn.playbtn(v-if='status !== 1' @click='start' pill)
                img(:src='require("../assets/icon-play--orange.svg")')
              b-btn.playbtn(v-if='status === 1' @click='pause' pill)
                img(:src='require("../assets/icon-pause--orange.svg")')
              b-btn.circle(@click='finish(true)' pill)
                img(:src='require("../assets/icon-delete.svg")')
        b-col.mt-auto.d-flex.justify-content-end.align-items-end.ml-auto(cols='6')
          router-link(to='/')
            img.img-position(:src='require("../assets/tomato--orange.svg")')
</template>
<script>
const format = function (value) {
  return value < 10 ? '0' + value : value
}
const today = {
  month: new Date().getMonth() + 1,
  date: new Date().getDate()
}
export default {
  name: 'Analysis',
  data () {
    return {
      nowdate: new Date(),
      series: [
        {
          name: '已完成事項',
          data: [7, 3, 5, 12, 8, 10, 9]
        }
      ],
      options: {
        chart: {
          height: 450,
          type: 'bar',
          events: {
            click: function (chart, w, e) {
              // console.log(chart, w, e)
            }
          }
        },
        colors: ['#6C9460', '#6C9460', '#6C9460', '#6C9460', '#6C9460', '#6C9460', '#F08448'],
        plotOptions: {
          bar: {
            columnWidth: '45%',
            distributed: true
          }
        },
        dataLabels: {
          enabled: false
        },
        legend: {
          show: false
        },
        xaxis: {
          categories: [
            [`${today.month}/${today.date - 7}`],
            [`${today.month}/${today.date - 6}`],
            [`${today.month}/${today.date - 5}`],
            [`${today.month}/${today.date - 4}`],
            [`${today.month}/${today.date - 3}`],
            [`${today.month}/${today.date - 2}`],
            [`${today.month}/${today.date - 1}`],
            [`${today.month}/${today.date}`]
          ],
          labels: {
            style: {
              colors: '#E8E8E8',
              fontSize: '12px'
            }
          }
        },
        yaxis: {
          tickAmount: 8,
          min: 0,
          labels: {
            style: {
              colors: '#E8E8E8',
              fontSize: '12px'
            }
          },
          axisBorder: {
            show: true,
            color: '#E8E8E8',
            offsetX: -1,
            offsetY: 0
          }
        }
      }
    }
  },
  filters: {
    dateformat: function (nowdate) {
      const year = format(nowdate.getFullYear())
      const month = format(nowdate.getMonth() + 1)
      const date = format(nowdate.getDate())
      return `${year}.${month}.${date}`
    },
    datformat: function (nowdate) {
      const year = format(nowdate.getFullYear())
      const month = format(nowdate.getMonth() + 1)
      const date = format(nowdate.getDate() - 7)
      return `${year}.${month}.${date}`
    },
    timeformat: function (nowdate) {
      const week = ['星期日', '星期一', '星期二', '星期三', '星期四', '星期五', '星期六']
      const hours = format(nowdate.getHours())
      const minutes = format(nowdate.getMinutes())
      const day = parseInt(format(nowdate.getDay()), 0)
      return `${week[day]} ${hours}:${minutes}`
    }
  },
  mounted () {
    const that = this
    this.timer = setInterval(function () {
      that.nowdate = new Date()
    }, 1000)
    this.renderChart(this.chartdata, this.options)
  },
  beforeDestroy () {
    if (this.timer) {
      clearInterval(this.timer)
    }
  },
  computed: {
    timeleft () {
      return this.$store.state.timeleft
    },
    timeText () {
      const m = Math.floor(this.timeleft / 60)
      const s = Math.floor(this.timeleft % 60)
      return `${m} : ${s}`
    }
  }
}
</script>
