<template>
  <table class="board">
    <tbody>
      <tr class="row" v-for="(row, i) in board" :key="i">
        <td class="col" :style="{width: cellHeight + 'px', height: cellHeight + 'px'}" v-for="(item, j) in row" :key="j">
          <Cell :value="item"></Cell>
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
      rows: 10,
      cols: 10,
      arr: [],
    }
  },
  computed: {
    board() {
      for(let i = 0; i < this.rows; i++){
        var rows = [];
        for(let j = 0; j < this.cols; j++){
          rows.push(0);
        }
        this.arr.push(rows);
      }
      var bombsCount = 0;
      while(bombsCount <= this.bombsNumber){
        var self = this;
        var x = self.random(self.rows);
        var y = self.random(self.cols);
        if(this.arr[x][y] != 9){
          this.arr[x][y] = 9;

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
      return this.arr;
    },
    cellHeight() {
      return this.gameWidth / this.cols;
    }
  },
  methods: {
    random(max) {
      return Math.floor(Math.random() * Math.floor(max));
    },
    increaseCount(x, y){
      if(0 <= x && x < this.rows && 0 <= y && y < this.cols){
        if(this.arr[x][y] != 9){
          this.arr[x][y]++;
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
  background-color: rgb(212, 212, 212);
  border: 2px solid rgb(180, 180, 180);
}
</style>