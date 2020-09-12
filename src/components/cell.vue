<template>
  <div class="cell" @click="handleClick">
    <span v-if="cellOwner !== -1">{{cellOwner}}</span>
  </div>
</template>

<script>
export default {
  name: 'cell',
  props: {
    row: Number,
    col: Number,
    players: Array,
    playerIndex: Number,
    winner: Boolean
  },
  data(){
    return {
      cellOwner: -1
    }
  },
  methods: {
    handleClick(){
      if(this.cellOwner === -1 && this.winner === false){
        this.cellOwner = this.players[this.playerIndex]
        this.$emit('cellSelected', {cellOwner:this.cellOwner, row: this.row, col:this.col})
      }
    }
  },
  mounted(){
    this.$emit('cellCreated', {cellOwner:this.cellOwner, row: this.row, col:this.col})
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
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
