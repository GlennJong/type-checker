<template>
  <div id="app">
    <TypeChecker></TypeChecker>
  </div>
</template>


<script>
import TypeChecker from './components/TypeChecker.vue';

export default {
  name: 'app',
  components: {
    TypeChecker
  },
  mounted() {
  },
}
</script>

<style>
body, html {
  margin: 0;
  padding: 0;
}
#app {
  display: flex;
  position: absolute;
  top: 0;
  left: 0;
  margin: 0;
  padding: 0;
  width: 100%;
  height: 100%;
  font-family: 'Noto Sans TC', 'Pingfang TC', '微軟正黑體', 'Microsoft JhengHei', sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
}
</style>
