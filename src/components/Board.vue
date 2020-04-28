<template>
  <table class="board">
    <tbody>
      <tr class="row" v-for="(row, i) in board" :key="i">
        <td class="col" :style="{width: cellHeight + 'px', height: cellHeight + 'px'}" v-for="(item, j) in row" :key="j" @click="openCell(i, j)" @contextmenu="flag($event, i, j)">
          <Cell :value="item" :state="state[i][j]"></Cell>
        </td>
      </tr>
    </tbody>
  </table>
</template>

<script>
import Cell from './Cell';
export default {
  name: 'board',
  components: {
    Cell
  },
  props: {
    gameWidth: {
      type: Number,
      default: 500
    },
    bombsNumber: {
      type: Number,
      required: true,
    }
  },
  data() {
    return {
      rows: 20,
      cols: 20,
      board: [],
      state: [],
      remain: 0,
    }
  },
  created() {
    for(let i = 0; i < this.rows; i++){
      var row = [];
      for(let j = 0; j < this.cols; j++){
        row.push(0);
      }
      this.board.push(row);
    }
    for(let i = 0; i < this.rows; i++){
      var row = [];
      for(let j = 0; j < this.cols; j++){
        row.push(0);
      }
      this.state.push(row);
    }
    var bombsCount = 0;
    while(bombsCount < this.bombsNumber){
      var self = this;
      var x = self.random(self.rows);
      var y = self.random(self.cols);
      if(this.board[x][y] != 9){
        this.board[x][y] = 9;

        self.increaseCount(x-1, y-1);
        self.increaseCount(x-1, y);
        self.increaseCount(x-1, y+1);

        self.increaseCount(x, y-1);
        self.increaseCount(x, y+1);

        self.increaseCount(x+1, y-1);
        self.increaseCount(x+1, y);
        self.increaseCount(x+1, y+1);
        bombsCount++;
      }
    }
    this.remain = this.rows * this.cols - this.bombsNumber;
  },
  computed: {
    cellHeight() {
      return this.gameWidth / this.cols;
    }
  },
  methods: {
    newGame() {
      for(let i = 0; i < this.rows; i++){
        for(let j = 0; j < this.cols; j++){
          this.board[i][j] = 0;
          this.state[i][j] = 0;
        }
      }
      var bombsCount = 0;
      while(bombsCount < this.bombsNumber){
        var self = this;
        var x = self.random(self.rows);
        var y = self.random(self.cols);
        if(this.board[x][y] != 9){
          this.board[x][y] = 9;

          self.increaseCount(x-1, y-1);
          self.increaseCount(x-1, y);
          self.increaseCount(x-1, y+1);

          self.increaseCount(x, y-1);
          self.increaseCount(x, y+1);

          self.increaseCount(x+1, y-1);
          self.increaseCount(x+1, y);
          self.increaseCount(x+1, y+1);
          bombsCount++;
        }
      }
      this.remain = this.rows * this.cols - this.bombsNumber;
      this.$forceUpdate();
    },
    random(max) {
      return Math.floor(Math.random() * Math.floor(max));
    },
    increaseCount(x, y){
      if(0 <= x && x < this.rows && 0 <= y && y < this.cols){
        if(this.board[x][y] != 9){
          this.board[x][y]++;
        }
      }
    },
    openCell(x, y){
      if(this.state[x][y] == 2) return;
      if(this.board[x][y] == 9){
        for(let i = 0; i < this.rows; i++){
          for(let j = 0; j < this.cols; j++){
            if(this.state[i][j] == 0) {
              this.state[i][j] = 1;
            }
          }
        }
        this.$forceUpdate();
        alert('Game Over!');
      }
      if(this.board[x][y] == 0){
        this.expand(x, y);
      } else {
        this.state[x][y] = 1;
        this.remain--;
      }
      this.$forceUpdate();
      if(this.remain == 0){
        alert("Winner!");
      }
    },
    flag(e, x, y){
      e.preventDefault();
      var state = this.state[x][y];
      if(state == 0) {
        this.state[x][y] = 2;
      }
      if(state == 2) {
        this.state[x][y] = 0;
      }
      this.$forceUpdate();
      console.log("flagged");
    },
    expand(x, y){
      if(0 <= x && x < this.rows && 0 <= y && y < this.cols){
        if(this.state[x][y] == 1) return;
        if(this.board[x][y] != 9){
          this.state[x][y] = 1;
          this.remain--;
          if(this.board[x][y] == 0){
            this.expand(x-1, y-1);
            this.expand(x-1, y);
            this.expand(x-1, y+1);
            this.expand(x, y-1);
            this.expand(x, y+1);
            this.expand(x+1, y-1);
            this.expand(x+1, y);
            this.expand(x+1, y+1);
          }
        }
      }
    }
  }
}
</script>

<style lang="scss">
.board{
  border-collapse: collapse;
  width: 100%;
}
.col{
  border: 2px solid rgb(180, 180, 180);
  padding: 0;
}
</style>