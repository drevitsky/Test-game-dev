<template>
    <div class="board--game">
      <div class="row">
        <div class="col-4">
          <Select 
          @changeMode='changeMode' 
          :gameModes='gameModes' 
          :isStart='isStart' />
        </div>
        <div class="col-4">
            <input 
            class="input-name" 
            type="text" 
            v-model="playerName" 
            placeholder="Enter your name" 
            :disabled='isStart'>
        </div>
        <div class="col-4">
            <button v-if="isStart" class="btn btn-secondary w-100" @click="stopGame()">STOP</button>
            <button v-else class="btn btn-secondary w-100" @click="playGame()">{{ isEndGame ? 'PLAY AGAIN' : 'PLAY' }}</button>
        </div>
      </div>
      
      <div class="row  mb-3 message-row" >
        <transition name="scale">
        <div class="col text-center message-col" v-if='isEndGame'>
          {{isWin ?'Congratulations ' : 'Sorry '}}{{ playerName }} you {{ isWin ? 'won': 'lose'}}
        </div>
        </transition>
        <transition name="fade">
        <div v-if='messageError' class="col text-center message-col" >
          {{ messageError }}
        </div>
        </transition>
      </div> 
      <Board 
      :field="field" 
      :isStart="isStart" 
      ref="childBoard" 
      :delay="delay"
      @EndGame='EndGame'
      />
    </div>
</template>

<script>
import axios from "axios";
import Select from '~/components/Select.vue'
import Board from '~/components/Board.vue'

  export default {
    components: {
    Select,
    Board
    },
    data() {
      return {
        playerName: "",
        field: 5,
        gameModes:[],
        delay: 0,
        currentMode: '',
        isStart: false,
        isEndGame:false,
        isWin: false,
        messageError: '',
        winnerName:''
      }      
    },
    created() {
      this.getGameSetting()
    },
    methods: {
      playGame() { 
        if (!this.currentMode) {
          this.messageError = 'Please select Game mode'
        } 
        if (this.playerName.trim() === "") {
          this.messageError = 'Please enter your name'
        }       
        if (this.currentMode && this.playerName.trim() !== "") {
            this.messageError = ''
            this.isStart = true
            this.$refs.childBoard.playGame()
        }
        return true
      },
      stopGame() {
        this.isStart = false
        this.$refs.childBoard.stopGame()
      },
      async getGameSetting () {
        try {
          const url = "https://starnavi-frontend-test-task.herokuapp.com/game-settings"
          const response = await axios.get(url).then(response => {
            this.gameModes = response.data;
          });
        }
        catch(e) {
          console.log('Error Axios game-settings', e)
          return e
        }
      },
      changeMode (data) {
        if (this.playerName.trim() === "") {
          this.messageError = 'Please enter your name'
        }else {
          this.messageError = ''
          }
        this.currentMode = data.changeMode
        if (this.currentMode){
          this.field = this.gameModes[this.currentMode].field
          this.delay = this.gameModes[this.currentMode].delay
          }
      },
      EndGame (data) {
        this.isEndGame = data.EndGame
        if (this.isEndGame){
          this.isStart = false
          }
        this.isWin = data.Win
        if (this.isWin) {
          this.winnerName = this.playerName
        }
        else {
          this.winnerName = 'computer'
        }
        if (data.Send) {
          this.sendResult(this.winnerName)
          setTimeout(() => {
            this.$nuxt.$emit('updateWinner',{
              updateWinner: true
            })
          }, 2000);
        }
      },
      sendResult(name) {
        const url = 'https://starnavi-frontend-test-task.herokuapp.com/winners'
        const date = this.setTime()
        axios.post(url, {
          date: date,
          winner: name
        })
        .then(function (response) {
          // console.log(response);
        })
        .catch(function (error) {
          console.log(error);
        });
      },
      setTime() {
        const date = new Date()
        let hours = this.toDobleDigit(date.getHours())
        let minutes = this.toDobleDigit(date.getMinutes())
        let seconds = this.toDobleDigit(date.getSeconds())
        let year = date.getFullYear()        
        let month = this.toDobleDigit(date.getMonth() + 1)
        let day = this.toDobleDigit(date.getDate())
        const fullDate = `${year}/${month}/${day} ${hours}:${minutes}:${seconds}`
        return fullDate
        },
        toDobleDigit(num) {
          if (num < 10){
            return '0' + num
            }
          else return String(num) 
        }
    }
  }
</script>

<style lang="scss" scoped>
  .board--game {
    padding: 30px;
  }
  .message {
    &-row {
      font-size: 24px;
      min-height: 36px;
    }
  }
  .input-name {
    display: inline-block;
    width: 100%;
    height: calc(1.5em + 0.75rem + 2px);
    padding: 0.375rem 1.75rem 0.375rem 0.75rem;
    font-size: 1rem;
    font-weight: 400;
    line-height: 1.5;
    color: #495057;
    vertical-align: middle;
    border: 1px solid #ced4da;
    border-radius: 0.25rem;
    background-color: #eeee;
    
  }
  .row-description {
    min-height: 24px;
  }
  .btn-secondary {
    &:hover {
      background-color: #5559;
    }
  }

  .fade-enter-active, .fade-leave-active {
    transition: opacity 1s;
  }
  .fade-enter, .fade-leave-to {
    opacity: 0;
  }
  .scale-enter-active, .scale-leave-active {
    transition: all 1s;
  }
  .scale-enter, .scale-leave-to {
    transform: scale(0);
  }
</style>