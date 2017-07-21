<template>
  <div id="app">
    <div class="header">
      <a href="https://github.com/paybase/" target="_blank"><img src="./assets/paybase.png"></a>
      <div>
        <h1>Pull Request Dashboard</h1>
        <input v-model="filter" type="text" placeholder="Filter by repo name or dev" autofocus />
      </div>
    </div>
    <dashboard :filter="filter"></dashboard>
  </div>
</template>

<script>
import Dashboard from './components/dashboard/Dashboard.vue';
export default {
  name: 'app',
  components: { Dashboard },
  data() {
    return {
      filter: '',
    };
  },
}
</script>

<style>
body {
  margin: 0;
  padding: 0;
}
#app {
  font-family: 'Josefin Sans', sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
}
input {
  font-size: 14px;
  padding: 6px 4px 4px 4px;
  min-width: 160px;
  font-family: 'Josefin Sans', sans-serif;
  outline-color: #fedd00;
  outline-width: 3px;
}
.header {
  z-index: 2;
  background-color: white;
  padding: 24px;
  display: flex;
  align-items: flex-end;
  flex-direction: row;
  position: sticky;
  top: 0;
  border-bottom: 2px solid #fedd00;
}
.header div {
  margin-left: 20px;
  text-align: left;
}
h1 {
  margin: 0;
  margin-bottom: 8px;
  font-size: 22px;
}
img {
  height: 70px;
}
</style>
