<template lang="pug">
#list.bg-green.w-100
  b-row.ml-2
    b-col.vh-100.boxshadow(cols=5)
      div.d-flex.mt-5
        p.text-list.mr-auto 待辦清單
        p.text-sis.mr-5 未完成
        p.text-sis.mr-3 已完成
      b-col(cols=12)
        div.position-relative
          b-form-input.inputcircle(v-model='newitem' :state='state' trim @keydown.enter='additem' placeholder='新增代辦事項')
          b-btn.btn-list(@click='additem' pill) +
        br
        b-row
          b-col(cols='12')
             b-table(:items='list' :fields='listfields' class="text-secondary")
              template(#cell(name)='data')
                b-form-input(
              v-if='data.item.edit'
              v-model='data.item.model'
              trim
              :state='data.item.state'
              @keydown.enter='changelist(data.index)'
              @keydown.esc='cancellist(data.index)'
            )
                span(v-else) {{ data.value }}
              template(#cell(action)='data')
                span(v-if='!data.item.edit')
                  b-btn(@click='editlist(data.index)')
                    img(:src='require("../assets/icon-edit.svg")')
                  b-btn(@click='dellist(data.index)')
                    img(:src='require("../assets/icon-delete.svg")')
                span(v-else)
                  b-btn(@click='changelist(data.index)')
                    font-awesome-icon(:icon='["fas", "check"]')
                  b-btn(@click='cancellist(data.index)')
                    font-awesome-icon(:icon='["fas", "undo"]')
          b-col(cols='6')
            b-table-simple
                tr(v-for='(item, idx) in finished' :key='idx')
                  td {{ item }}
                  td
                    b-btn(@click='delfinish(idx)')
                      font-awesome-icon(:icon='["fas", "trash"]')
    b-col(cols='6').w-100
      b-row.text-center
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
export default {
  name: 'List',
  data () {
    return {
      newitem: '',
      listfields: [
        { key: 'name', label: '名稱' },
        { key: 'action', label: '操作' }
      ]
    }
  },
  computed: {
    state () {
      if (this.newitem.length === 0) {
        return null
      } else if (this.newitem.length < 2) {
        return false
      } else {
        return true
      }
    },
    list () {
      return this.$store.getters.list.map(item => {
        if (item.model.length < 2) {
          item.state = false
        } else {
          item.state = true
        }
        return item
      })
    },
    finished () {
      return this.$store.state.finished
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
    additem () {
      if (this.state) {
        this.$store.commit('addList', this.newitem)
        this.newitem = ''
      } else {
        this.$swal({
          icon: 'error',
          title: '錯誤',
          text: '必須要兩個字以上'
        })
      }
    },
    editlist (index) {
      this.$store.commit('editList', index)
    },
    changelist (index) {
      console.log(index)
      if (this.list[index].state) {
        this.$store.commit('changeList', index)
      }
    },
    cancellist (index) {
      this.$store.commit('cancelList', index)
    },
    dellist (index) {
      this.$store.commit('delList', index)
    },
    delfinish (index) {
      this.$store.commit('delFinish', index)
    }
  }
}
</script>
