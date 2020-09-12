<template>
  <div id="app">
    <h2>Tic Tac Toe</h2>
    <h3 v-if="!winner && cat === false">Player {{ players[playerIndex] }}'s turn</h3>
    <h3 v-if="cat === true">Cat Game</h3>
    <h3 v-if="winner">Player {{ players[playerIndex] }} is the winner!</h3>
    <div class="grid">
      <cell
        v-for="n in 9"
        :row="getRow(n)"
        :col="getCol(n)"
        :key="n"
        v-on:cellCreated="addCell"
        v-on:cellSelected="updateCell"
        :playerIndex="playerIndex"
        :players="players"
        :winner="winner"
      />
    </div>
    <button @click="reset">new game</button>
  </div>
</template>

<script>
import cell from './components/cell.vue'

export default {
  name: 'app',
  components: {
    cell
  },
  data(){
    return {
      playerIndex: 0,
      players:['o','x'],
      cells: [],
      winner: false,
      cat: false
    }
  },
  methods: {
    getCol(n){
      if(n <= 3){
        return n
      }else if(n > 3 && n < 7){
        return n-3
      }else {
        return n-6
      }
    },
    getRow(n){
      return Math.ceil(n/3)
    },
    addCell(cell){
      console.log(cell);
      this.cells.push(cell)
    },
    updateCell(update){
      let theseCells = this.cells
      let indexToCell = this.cells.findIndex(cell => cell.row === update.row && cell.col === update.col)
      console.log(indexToCell);
      theseCells.splice(indexToCell, 1, update)
      this.$set(this.cells,theseCells)
      this.checkWin()
      if(!this.winner){
        this.playerIndex === 0 ? this.playerIndex++ : this.playerIndex = 0
      }
    },
    checkWin(){
      let winnerFound = false
      // check each row
      for(let row = 1; row < 4; row++){
        let cellRow = this.cells.filter(cell => cell.row === row)
        console.log(this.allCellsAreSame(cellRow));
        if(this.allCellsAreSame(cellRow) === true){
          winnerFound = true
        }
      }
      for(let col = 1; col < 4; col++){
        let cellCol = this.cells.filter(cell => cell.col === col)
        if(this.allCellsAreSame(cellCol) === true){
          winnerFound = true
        }
      }
      //check diagonals
      let leftDiagonalCells = [this.cells[0],this.cells[4],this.cells[8]]
      if(this.allCellsAreSame(leftDiagonalCells) === true){
        winnerFound = true
      }
      let rightDiagonalCells = [this.cells[2],this.cells[4],this.cells[6]]
      if(this.allCellsAreSame(rightDiagonalCells) === true){
        winnerFound = true
      }
      if(winnerFound){
        this.winner = true
      }else if(this.cells.filter(cell => cell.cellOwner === -1).length === 0){
        this.cat = true
      }
    },
    allCellsAreSame(cells){
      return cells.every(cell => cell.cellOwner === this.players[this.playerIndex])
    },
    reset(){
      window.location.reload()
    }
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
.grid {
  display: grid;
  grid-template-columns: repeat(3, max-content);
  justify-content: center;
}
.cell {
  width: 24px;
  height: 24px;
  border: 2px solid black;
}
button {
  margin-top: 15px;
}
</style>
