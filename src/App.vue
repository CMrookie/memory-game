<template>
  <div id="app">
    <!-- <router-view/> -->
    <Game></Game>
  </div>
</template>

<script>
import Game from '@/views/Game.vue';
export default {
  components: {
    Game
  }
}
</script>

<style lang="scss">
@import '@/assets/styles/based.scss';

body{
  width: 750px;
  height: 100vh;
  display: flex;
  background: #060ab2;
}
#app{
  width: 750px;
  height: 100vh;
  display: flex;
}
// #app {
//   font-family: 'Avenir', Helvetica, Arial, sans-serif;
//   -webkit-font-smoothing: antialiased;
//   -moz-osx-font-smoothing: grayscale;
//   text-align: center;
//   color: #2c3e50;
// }
// #nav {
//   padding: 30px;
//   a {
//     font-weight: bold;
//     color: #2c3e50;
//     &.router-link-exact-active {
//       color: #42b983;
//     }
//   }
// }
</style>
