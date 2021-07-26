<template>
  <div id="app">
    <UploadImg />
  </div>
</template>

<script>
import UploadImg from './components/UploadImg.vue'

export default {
  name: 'App',
  components: {
    UploadImg
  }
}
</script>

<style lang="sass">
html, body
  margin: 0
  padding: 0
*
  // outline: solid 1px #000
#app
  font-family: Avenir, Helvetica, Arial, sans-serif
  -webkit-font-smoothing: antialiased
  -moz-osx-font-smoothing: grayscale
  text-align: center
  color: #2c3e50
  margin-top: 60px
</style>
