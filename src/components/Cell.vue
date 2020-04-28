<template>
  <div class="cell" :class="{opened: state == 1}" :style="{color: color(value)}">
    <div v-if="state == 1">
      <div v-if="value == 9" class="bomb">
        <img src="src/assets/bomb.png" alt="">
      </div>
      <div v-else-if="value != 0">{{value}}</div>
    </div>
    <div v-if="state == 2">
      cờ
    </div>
  </div>
</template>

<script>
export default {
  name: 'cell',
  props: {
    value: {
      type: Number,
      required: true,
    },
    state: {
      type: Number,
      required: true,
    }
  },
  data() {
    return {
      
    }
  },
  methods: {
    color(value){
      switch (value) {
        case 1:
          return '#0400ff'
        case 2: 
          return '#13a324'
        case 3: 
          return '#ed0000'
        case 4: 
          return '#05037a'
        case 5: 
          return '#6b0707'
        case 6: 
          return '#efa5a5'
        case 7: 
          return '#fcfc20'
        default:
          break;
      }
    }
  }
}
</script>

<style lang="scss">
.cell{
  display: flex;
  justify-content: center;
  align-content: center;
  cursor: pointer;
  height: 100%;
  font-weight: bold;
  background-color: rgb(129, 129, 129);
  div{
    display: flex;
    text-align: center;
    align-items: center;
    font-size: 30px;
    img{
      width: 100%;
    }
  }
}
.opened {
  background-color: rgb(212, 212, 212);
}
.bomb{
  padding: 7px;
}
</style>