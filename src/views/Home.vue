<template lang="pug">
#home.w-100.bg-green
    b-container
      b-row.vh-100.mx-0
        b-col(cols='12')
          div.text-right
            p {{  dateformat }}
        b-col(cols='12')
          b-row.text-center
            b-col(cols='6')
              div.mb-0
                p.text-time {{ timeText }}
                div.d-flex.align-items-center.ml-5
                  b-btn.circle(to='/settings' pill)
                    img(:src='require("../assets/icon-bell.svg")')
                  b-btn.playbtn(v-if='status !== 1' @click='start' pill)
                    img(:src='require("../assets/icon-play--orange.svg")')
                  b-btn.playbtn(v-if='status === 1' @click='pause' pill)
                    img(:src='require("../assets/icon-pause--orange.svg")')
                  b-btn.circle(@click='finish(true)' pill)
                    img(:src='require("../assets/icon-delete.svg")')
            b-col.col-sm-6.text-left(cols='12')
              div.pt-5.pb-2
                ul
                  template(v-for='(item,index) in list')
                    li.my-3
                      div.d-flex.align-items-center
                        span.mr-3.taskcircle
                        span.mr-auto.taskText {{ item }}
                      div.smcircleLeft(v-if='index === 0')
                        span.smcircle
                        span.smcircle.ml-1
                        span.smcircle.ml-1
                        span.smcircle1.ml-1
        b-col.mt-auto.d-flex.justify-content-center.align-items-end(cols='12')
          router-link(to='/')
            img(:src='require("../assets/tomato--orange.svg")')
</template>

<script>

export default {
  name: 'Home',
  data () {
    return {
      list: ['學習 Vue 切版', '練習番茄鐘', '只有部分功能', '其他功能沒有寫'],
      timer: 0
    }
  },
  computed: {
    status () {
      return this.$store.state.status
    },
    // list () {
    //   return this.$store.state.list
    // },
    current () {
      return this.$store.state.current
    },
    currentText () {
      let text = this.current
      if (text.length === 0) {
        if (this.list.length === 0) {
          text = '沒有事項'
        } else {
          text = '點擊開始'
        }
      }
      return text
    },
    timeleft () {
      return this.$store.state.timeleft
    },
    timeText () {
      const m = Math.floor(this.timeleft / 60)
      const s = Math.floor(this.timeleft % 60)
      return `${m} : ${s}`
    }
  },
  methods: {
    pause () {
      clearInterval(this.timer)
      this.$store.commit('changeStatus', 2)
    },
    start () {
      if (this.status !== 2 && this.list.length > 0) {
        this.$store.commit('start')
      }
      if (this.current.length > 0) {
        this.$store.commit('changeStatus', 1)
        this.timer = setInterval(() => {
          this.$store.commit('countdown')
          if (this.timeleft <= -1) {
            this.finish(false)
          }
        }, 1000)
      }
    },
    finish (skip) {
      clearInterval(this.timer)
      this.$store.commit('changeStatus', 0)
      this.$store.commit('addFinish')

      if (!skip) {
        const audio = new Audio()
        audio.src = require('../assets/' + this.$store.state.sound)
        audio.play()
      }

      if (this.list.length > 0) {
        this.start()
      } else {
        this.$swal({
          icon: 'success',
          title: '結束'
        })
      }
    }
  }
}
</script>
