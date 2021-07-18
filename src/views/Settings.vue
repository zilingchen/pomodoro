<template lang="pug">
#settings
  body(:style='mybgb')
    b-container
      b-table(:items='items' :fields='fields' @row-clicked='select')
        template(#cell(src)='data')
          audio(controls :src='require("../assets/"+data.value)')
        template(#cell(select)='data')
          font-awesome-icon(:icon='["fas", "check"]' v-if='sound === data.item.src')
</template>

<script>
export default {
  name: 'Settings',
  data () {
    return {
      mybgb: {
        background: 'linear-gradient(to right, #6c757d 10%, #FAF0E6 10%)',
        height: '60vh'
      },
      items: [
        { name: '鈴聲', src: 'alarm.mp3' },
        { name: 'Yay', src: 'yay.mp3' }
      ],
      fields: [
        { key: 'name', label: '名稱' },
        { key: 'src', label: '試聽' },
        { key: 'select', label: '選擇' }
      ]
    }
  },
  computed: {
    sound () {
      return this.$store.state.sound
    }
  },
  methods: {
    select (item) {
      this.$store.commit('selectSound', item.src)
    }
  }
}
</script>
