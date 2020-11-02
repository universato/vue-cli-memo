<template>
  <Memos app_title= 'Vue CLI Memo app(SPA)' />
</template>

<script>
import Memos from './components/Memos.vue'

export default {
  name: 'App',
  components: {
    Memos,
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

body {
  background-color: #eee;
}
</style>
