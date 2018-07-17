<template>
  <div id="app">
    <h1>vue recycler view</h1>
    <list />
  </div>
</template>

<script>
import List from "./list"
export default {
  name: "app",
  components: { List }
}
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

* {
  padding: 0;
  margin: 0;
}
</style>
