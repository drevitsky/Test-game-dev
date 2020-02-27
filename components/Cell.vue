<template>
    <div class="cellwrap" :style="getCellStyle()" :class="addClass()" >{{ keyId }}</div>
</template>

<script>
    export default {
        props: [ 'keyId', 'cellSize','cell' ],
        methods: {
            getCellStyle() {
              return `width: ${ this.cellSize }px; height: ${ this.cellSize }px;`
             },
            addClass() {
              // console.log('CELL.vue: this.cell', this.cell)
              if (this.cell.isHighlighted)
              return 'isHighlighted'
              else if (this.cell.isClicked)
              return 'isClicked'
              else if (this.cell.isFail)
              return 'isFail' 
            }
        }
        
    }
</script>

<style scoped>
.cellwrap {
    
    border: 2px solid #eeee;
    border-radius: 2px;
    display: inline-block;
    text-align: center;
    vertical-align: center;
    line-height: 25px;
    font-size: 10px;
}
.isFail {
  background-color: red;
}
.isHighlighted {
  background-color: blue;
}
.isClicked {
  background-color: green;
}

</style>