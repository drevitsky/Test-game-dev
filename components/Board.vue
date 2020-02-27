<template>
  <div>
    <div class="board" :style="getBoardStyle()">
      <transition-group name="cell" tag="div">
        <Cell 
        v-for="cell in cells"
        :key="cell.id"
        :keyId="cell.id"
        :cellSize="cellSize"
        @click.native.prevent="clickCell(cell)"        
        :class="cellStyle"
        :cell='cell'               
        />
        </transition-group>
    </div>
  </div>
</template>

<script>
import Cell from '~/components/Cell.vue'
  export default {
    props: [ 'field', 'isStart','delay' ],
    data() {
        return { 
          cells:[],
          cellStyle:'',
          human:0,
          skynet:0,
          isEndGame: false,
          isWin:false,
          isStop: false,
          isSend: false
        }
    },
    mounted() {      
      this.initCells()      
    },
    components: {
        Cell
    },
    methods: {
        getBoardStyle: function() {            
            return `width: ${ this.widthBoard }px`
        },
        clickCell: function(cell) {
          if (cell.isHighlighted) {
            cell.isHighlighted = false
            cell.isClicked = true
            return
          }
          return
        },
        initCells: function () {
          const arrCells = [];
          let boardSize = this.boardSize()
          for (let i = 0; i < boardSize; i++) {
            arrCells.push({
              id: i,
              isHighlighted: false,
              isClicked: false,
              isFail: false             
            });
          }
          this.cells = arrCells;
        },
        boardSize: function() {
            return this.field*this.field
        },
        playGame() {
          this.reset()
          this.isStop = false
          const delay = this.delay
          const cellsLength = this.boardSize()
          const randomCell = this.getRandomCell( 0, cellsLength )
          const cells = this.cells
          const testClick = this.testClick
          const endGame = this.endGame
          let testStop = this.testStop
          let end = 'false'
          let stop = false
          let index = 0
          let skynet = this.skynet
          let human = this.human
              
          setTimeout(function go() {
            cells[randomCell[index]].isHighlighted = true
            setTimeout(function() {
              cells[randomCell[index]].isHighlighted = false
              end = testClick(cells[randomCell[index]])
              stop = testStop()
                if (end === 'false'&& !stop){
                  if (index < cellsLength-1) { 
                    index++
                    setTimeout (go,0) 
                  }
                }
              else if (end === 'skynet') {
                endGame('skynet')
              }
              else if (end === 'human') {
                endGame('human')
              }
            }, delay)
          }, 0)
        },
        reset() {
          this.human = 0
          this.skynet = 0
          this.isEndGame = false
          this.isWin = false
          this.$emit('EndGame',{
            EndGame: this.isEndGame,
            Win: this.isWin
          })
          this.initCells()
        },
        stopGame() {
          this.isStop = true
          this.reset()
        },
        testStop() {
          if (this.isStop === true) {return true}
          else return false
        },
        testClick(data) {
          if(data.isClicked !== true){
            data.isFail = true
            this.skynet++
          } 
          else {
            this.human++
          }
          let cellsLength = this.boardSize()
          if(this.skynet > cellsLength/2) {
            this.isSend = true
              return 'skynet'
          }
          if(this.human > cellsLength/2) {
            this.isSend = true
              return 'human'
          }
          return 'false'
        },
        
        endGame(data) {
          if (data === 'skynet') {
          } else if (data === 'human') {
            this.isWin = true
          }
          this.setIsEndGame()
        },
        setIsEndGame() {
          this.isEndGame = true
          this.$emit('EndGame',{
            EndGame: this.isEndGame,
            Win: this.isWin,
            Send: this.isSend
          })
        },
        getRandomCell(min, max) {
          const sourceArray = []
          const anotherArray = []
          let length = max - min
          let index = 0
          while (length--) {
            let a = sourceArray.push(length + min)
          }
          while (sourceArray.length) {
            index = Math.round(Math.random() * (sourceArray.length - 1))
            anotherArray.push(sourceArray[index]);
            sourceArray.splice(index, 1);
          }
          return anotherArray; 
        }
    },
    computed: {
        cellSize: function () {
          return 450/this.field
        },
        widthBoard: function() {
          return this.field*this.cellSize+4
        }
    },
    watch: {
      field() {
        this.initCells()
      }
    }
  }
</script>

<style scoped>
.board {
  margin: 0 auto; 
  border: 2px solid #eeee;; 
}
.cell-move {
  transition: transform 1s;
}

</style>