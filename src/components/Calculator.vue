<template>
  <div>
      <display :value="display"/>
      <div class="buttons">
        <Button label='LIMPAR' advanced extended @onCalc="clear"/>
        <Button label='<i class="fas fa-percentage"></i>' advanced @onCalc="setOperation('%')"/>
        <Button label='<i class="fas fa-divide"></i>' operation @onCalc="setOperation('/')"/>
        <Button label='7' @onCalc="add"/>
        <Button label='8' @onCalc="add"/>
        <Button label='9' @onCalc="add"/>
        <Button label='<i class="fas fa-times"></i>' operation @onCalc="setOperation('*')"/>
        <Button label='4' @onCalc="add"/>
        <Button label='5' @onCalc="add"/>
        <Button label='6' @onCalc="add"/>
        <Button label='<i class="fas fa-minus"></i>' operation @onCalc="setOperation('-')"/>
        <Button label='1' @onCalc="add"/>
        <Button label='2' @onCalc="add"/>
        <Button label='3' @onCalc="add"/>
        <Button label='<i class="fas fa-plus"></i>' operation @onCalc="setOperation('+')"/>
        <Button label='0' extended @onCalc="add"/>
        <Button label='.' @onCalc="add"/>
        <Button label='<i class="fas fa-equals"></i>' equals @onCalc="setOperation('=')"/>
      </div>
  </div>
</template>

<script>
import Display from './Display'
import Button from './Button'

export default {
  data() {
    return {
      display: '0',
      clearDisplay: false,
      operation: null,
      values: [0,0],
      current: 0
    }
  },
  components: { Display, Button },
  methods: {
    clear(){
      Object.assign(this.$data, this.$options.data())
    },
    setOperation(operation) {
      if(this.current === 0) {
        this.operation = operation
        this.current = 1
        this.clearDisplay = true
      }else{
        const equals = operation == '='
        const currentOperation = this.operation

        try{
          if(currentOperation != '%'){
            this.values[0] = eval(
              `${this.values[0]} ${currentOperation} ${this.values[1]}`
            )
          }else{
            this.values[0] = this.values[0]/100 * this.values[1]
          }
        }catch(e){
          this.$emit('onErro', e)
        }

        this.values[1] = 0
        this.display = this.values[0]
        this.operation = equals ? null : operation
        this.current = equals ? 0 : 1
        this.clearDisplay = !equals
      }
    },
    add(n){
      if(n === '.' && this.display.includes('.')){
        return
      }

      const clearDisplay = this.display === '0' || this.clearDisplay
      const currentValue = clearDisplay ? '' : this.display
      const displayValue  = currentValue + n

      this.display = displayValue
      this.clearDisplay = false
      this.values[this.current] = displayValue

    }
  }
}
</script>