<template lang="pug">
#home
  body(:style='mybg')
    b-container
      b-row(align-center)
        b-col(cols='6')
          b-btn(:style='mystyle' v-if='status !== 1' @click='start' pill)
            font-awesome-icon(:icon='["fas", "play"]')
          b-btn(variant='secondary' v-if='status === 1' @click='pause' pill)
           font-awesome-icon(:icon='["fas", "pause"]')
          h2(ml-auto) {{ timeText }}
          b-btn(bg='secondary' v-if='current.length > 0' @click='finish(true)')
            font-awesome-icon(:icon='["fas", "times"]')
        b-col(cols='6')
          b-span(class="rounded-circle border-dark" width='150px' height='150px')
          h1(class="text-secondary" display='inline-block') {{ currentText }}
</template>

<script>
export default {
  name: 'Home',
  data () {
    return {
      mybg: {
        background: 'linear-gradient(to right, #6c757d 10%, #FAF0E6 10%)',
        height: '60vh'
      },
      mystyle: {
        backgroundColor: '#FFB6C1'
      },
      timer: 0
    }
  },
  computed: {
    status () {
      return this.$store.state.status
    },
    list () {
      return this.$store.state.list
    },
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
