<template>
  <div id="app">
    <h1>We(a)t(h) City's</h1>
    <img alt="Vue logo" src="https://img.icons8.com/doodle/150/000000/summer.png">
    <search />
  </div>
</template>

<script>
import search from './components/search.vue'

export default {
  name: 'app',
  components: {
    search,
  }
}
</script>

<style>
img{
  margin-bottom: 10px;
}
@font-face{
  font-family: snaps;
  src: url(./assets/fonts/snaps.woff);
}
h1{
  font-family: snaps;
  font-size: 300%;
  color:white;
  margin-top: 10px;
  margin-bottom: 10px;
}
#app{
  text-align: center;
  color: #2c3e50;
  margin-top: 30px;
}
body{
  background-color: #20b6e8b7;
  background-image: url("./assets/fondMeteo.png");
  background-size: cover;
  background-repeat: no-repeat;
  background-position:center;
  background-attachment: fixed;
}
</style>
