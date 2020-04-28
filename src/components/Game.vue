<template>
  <div class="game" :style="{width: gameWidth + 'px'}">
    <div class="header">
      <div class="mineleft">000</div>
      <button class="main-btn" @click="newGame">New Game</button>
      <div class="time">00:00</div>
    </div>
    <Board
      :gameWidth=gameWidth
      :bombsNumber=bombsNumber
      ref="board"
      >
    </Board>
  </div>
</template>

<script>
import Board from './Board';
export default {
  name: 'game',
  components: {
    Board,
  },
  data() {
    return{
      gameWidth: 800,
      bombsNumber: 60,
    }
  },
  methods: {
    newGame() {
      this.$refs.board.newGame();
    }
  }
}
</script>

<style lang="scss">
.header{
  display: flex;
  justify-content: space-between;
  font-size: 24px;
  border: 1px solid black;
}
.mineleft{
  width: 100px;
  background-color: black;
  color: red;
  font-weight: bold;
}
.time{
  width: 100px;
  background-color: black;
  color: red;
  font-weight: bold;
}
</style>