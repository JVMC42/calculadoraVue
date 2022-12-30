<template>

  <div class="calculator">
    <MyDisplay :value ="displayValue"/>
    <MyButton label="AC" triple  @onClick ='clearMemory'/> 
    <MyButton label="/" operation @onClick ='setOperation'/> 
    <MyButton label="7" @onClick ='addDigit'/> 
    <MyButton label="8" @onClick ='addDigit'/> 
    <MyButton label="9" @onClick ='addDigit'/> 
    <MyButton label="*" operation  @onClick ='setOperation'/> 
    <MyButton label="4" @onClick ='addDigit'/> 
    <MyButton label="5" @onClick ='addDigit'/> 
    <MyButton label="6" @onClick ='addDigit'/> 
    <MyButton label="-" operation  @onClick ='setOperation'/> 
    <MyButton label="1" @onClick ='addDigit'/> 
    <MyButton label="2" @onClick ='addDigit'/> 
    <MyButton label="3" @onClick ='addDigit'/> 
    <MyButton label="+" operation  @onClick ='setOperation'/> 
    <MyButton label="0" double @onClick ='addDigit'/> 
    <MyButton label="." @onClick ='addDigit'/> 
    <MyButton label="=" operation  @onClick ='setOperation'/> 
  </div>

</template>

<script>

import MyDisplay from "../components/MyDisplay.vue"
import MyButton from "../components/MyButton.vue"

export default {
  data: function() {
    return {
      displayValue: "0",
      clearDisplay: false,
      operation: null,
      values: [0, 0],
      current: 0
    }
  },
    components: {MyButton,MyDisplay},
    methods: {
      clearMemory(){
        Object.assign(this.$data, this.$options.data())
      },
      setOperation(operation) {
        if (this.current === 0) {
          this.operation = operation
          this.current = 1
          this.clearDisplay = true
        }else {
          const equals = operation === '='
          const currentOperation = this.operation

          try {
            this.values[0] = eval(
              `${this.values[0]} ${currentOperation} ${this.values[1]}`)
              if (this.values[0] % 1 !== 0) this.values[0] = this.values[0].toFixed(5)
              if(isNaN(this.values[0]) || !isFinite(this.values[0])) {
                this.clearMemory()
                return
              }
          } catch(e) {
            this.$emit('onError', e)
          }

          this.values[1] = 0
          this.displayValue = this.values[0]
          this.operation = equals ? null : operation
          this.current = equals ? 0 : 1
          this.clearDisplay = !equals

        }
      },
      addDigit(d) {
        if (d === '.' && this.displayValue.includes('.')) {
          return
        }

        const clearDisplay = this.displayValue === '0' || this.clearDisplay
        const currentValue = clearDisplay ? "" : this.displayValue
        const displayValue = currentValue + d

        this.displayValue = displayValue
        this.clearDisplay = false

        if (d !== ".") {
          const i = this.current
          const newValue = parseFloat(displayValue)
          this.values[i] = newValue
        }
      }
    }
}

</script>

<style>

.calculator {
    height: 320px;
    width: 235px;
    border-radius: 5px;
    /* overflow: hidden; */

    display: grid;
    grid-template-columns: repeat(4,25%) ;
    grid-template-rows: 1fr 48px 48px 48px 48px 48px;
}

</style>