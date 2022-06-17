<template>
  <div id="app">
    <Header text="Pradeep Alex" />
    <hr>
    <Testing />
    <hr>
    <TestingFile />
    <hr>
    <HistoricalData/>
  </div>
</template>

<script>
import Header from './components/Header.vue'
import Testing from 'testing-file'
import TestingFile from 'testing-file1'
import HistoricalData from './plugins/HistoricalData/HistoricalData.vue'

export default {
  name: 'App',
  components: {
    Header,
    Testing,
    TestingFile,
    HistoricalData
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
  /* margin-top: 60px; */
}
</style>
