<template>
  <div>
    <b-form-select v-model="selected" class="mb-3" 
      @change='changeMode' :disabled='isStart'>
      <b-form-select-option :value="null">Pick game mode</b-form-select-option>
      <b-form-select-option 
      v-for="mode in Object.keys(gameModes)" 
      :key="mode" 
      :value="mode"
      >
      {{ mode }}
      </b-form-select-option>
    </b-form-select>
  </div>
</template>

<script>
import axios from "axios";
  export default {
    props:[ 'gameModes','isStart' ],
    data() {
      return {
        selected: null
      }
    },
    methods: {
      changeMode () {
        this.$emit('changeMode', {
          changeMode: this.selected
        })
      }
    }
  }
</script>
<style lang="scss" scoped>
  
</style>
