<template>
  <div id="app">
    <MyMemo />
  </div>
</template>

<script>
import  MyMemo from './components/MyMemo.vue'

export default {
  name: 'App',
  components: {
    MyMemo
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
  margin-top: 10px;
}
</style>
