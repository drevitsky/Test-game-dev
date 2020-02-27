<template>
    <div class="bg-color">
        <div class="row boards">
            <div class="col-md-6">                
                <BoardGame />
            </div>
            <div class="col-md-6">                
                <BoardLeader />
            </div>
        </div>

    </div>
</template>

<script>
import axios from "axios";
import BoardGame from '~/components/BoardGame.vue'
import BoardLeader from '~/components/BoardLeader.vue'

    export default {
      components: {
        BoardGame,
        BoardLeader
      }
    }
</script>

<style lang="scss" scoped>
.boards {
  min-height: 40vh;
  padding-top: 30px;
}
.col-sm-6 {
    outline: 1px solid #eeee;;
    min-width: 40vw;
}
.bg-color {
  background-color: #fff;
}

</style>
