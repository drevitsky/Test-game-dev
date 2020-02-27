<template>
  <div class="board--leader">
    <h2 class="board--leader_title">Leader Board</h2>
    <transition-group name="list" tag="div">
    <div class="winner" v-for='winner in winners' :key="winner.id" >
      <div class="winner__winner">{{winner.winner}}</div>
      <div class="winner__date">{{winner.date}}</div>
    </div>
    </transition-group>
  </div>
</template>

<script>
  import axios from "axios";
  
  export default {
    data() {
      return {
          winners: [],
          updateWinner:false
      }
    },
    created() {
        this.getWinners()
        this.$nuxt.$on('updateWinner', data => {
          this.updateWinner = data.updateWinner
        })
    },
    methods: {
      async getWinners () {
        try {
          const url = "https://starnavi-frontend-test-task.herokuapp.com/winners"
          const response = await axios.get(url).then(response => {              
              this.winners = response.data.reverse();              
          });
        }
        catch(e) {
        console.log('Error Axios getWinners ', e)
        return e
        }
      },
    },
    watch: {
      updateWinner() {        
        if (this.updateWinner === true){
            this.updateWinner = false
            this.getWinners()
        }
      }
    } 
        
  }
</script>

<style lang="scss" scoped>
.board--leader {
  padding: 30px;
  &_title {
    margin-bottom: 30px;
    color: #5559;
    font-size: 40px;
  }
}
.winner {
  display: flex;
  justify-content: space-between;
  background-color: #5555;
  margin-bottom: 5px;
  padding: 10px;
  max-width: 80%;
  color: #555;
  font-size: 20px;
}
.list-enter-active, .list-leave-active {
  transition: all 1s;
}
.list-enter, .list-leave-to {
  opacity: 0;
  transform: translateX(50px);
}

</style>