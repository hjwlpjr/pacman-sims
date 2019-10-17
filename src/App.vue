<template>
  <div id="app">
    <table>
    <tr v-for="(el, index) in rawWorld" :key="index">
      <td v-for="(char, charIndex) in el" :key="charIndex">{{ char }}</td>
    </tr>
    </table>
  </div>
</template>

<script>
import { map, obstacle, pacman } from './assets/world'
export default {
  name: 'app',
  created() {
    window.addEventListener('keyup', this.movePacman)
  },
  data() {
    return {
      obstacle: obstacle,
      lastPosition: {
        row: 17,
        col: 13
      },
      rawWorld: map
    }
  },
  methods: {
    async movePacman(e) {
      let expectedNewPosition = Object.assign({},this.lastPosition)
      if(e.keyCode === 37) {
        expectedNewPosition.col -= await 1
        const targetChar = this.rawWorld[expectedNewPosition.row][expectedNewPosition.col]
        if(this.obstacle.indexOf(targetChar) === -1) {
          this.rawWorld[this.lastPosition.row].splice(this.lastPosition.col, 1, ' ')
          this.rawWorld[expectedNewPosition.row].splice(expectedNewPosition.col, 1, pacman)
          this.lastPosition.col -= await 1
        }
      }

    },
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
