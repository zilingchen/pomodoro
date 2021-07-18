<template lang="pug">
#sidebar
    b-sidebar(:style='myside')
        b-btn(to='/' size='lg' :style='mtbtn')
          font-awesome-icon(:icon='["fas", "play-circle"]')
        b-btn(to='/list' size='lg' :style='mtbtn')
          font-awesome-icon(:icon='["fas", "check"]')
        b-btn(to='/settings' size='lg' :style='mtbtn')
          font-awesome-icon(:icon='["fas", "cog"]')
  keep-alive
</template>
<script>
export default {
  name: 'App',
  data () {
    return {
      mybtn: {
        display: 'block'
      },
      myside: {
        width: '150px',
        height: '100vh',
        backgroundColor: '#6c757d',
        textAlign: 'center',
        fontSize: '3rem'
      }
    }
  }
}
</script>
