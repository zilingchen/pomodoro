<template lang="pug">
#app.vh-100.flex-row.d-flex
  b-nav.justify-content-center.align-items-center.nav
    b-nav-item(to='/list')
      img(:src='require("./assets/icon-list.svg")')
    b-nav-item(to='/analysis')
      img(:src='require("./assets/icon-analysis.svg")')
  keep-show
  router-view
</template>
<script>
import './style/app.styl'

export default {
  name: 'App'
}
</script>
