<template>
  <div id="app">
    <h1>Vehicle Selection</h1>
    <VehicleDropdown />
  </div>
</template>

<script>

import VehicleDropdown from "./VehicleDropdown";
export default {
  name: 'HelloWorld',
  props: {
    msg: String
  },
  components: {
    VehicleDropdown,
  },
}
</script>
<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  text-align: center;
  margin-top: 2rem;
}
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
