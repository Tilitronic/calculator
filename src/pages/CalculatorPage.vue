<!-- eslint-disable @typescript-eslint/no-unsafe-call -->
<!-- eslint-disable @typescript-eslint/no-unsafe-member-access -->
<!-- eslint-disable @typescript-eslint/no-unsafe-assignment -->
<script lang="ts">
import { defineComponent, ref } from 'vue';
import ButtonComponent from 'src/components/ButtonComponent.vue';



export default defineComponent({
  name: 'CalculatorPage',
  components: { ButtonComponent },
  data(){
    return{
      inputValue: '',
      labelValue: '',
    }
  },
  calcButtons: [
    {
      label: '(',
      type: 'operator'
    },
    {
      label: ')',
      type: 'operator'
    },
    {
      label: '%',
      type: 'operator'
    },
    {
      label: 'C',
      type: 'operator'
    },
    {
      label: '7',
      type: 'number'
    },
    {
      label: '8',
      type: 'number'
    },
    {
      label: '9',
      type: 'number'
    },
    {
      label: '/',
      type: 'operator'
    },
    {
      label: '4',
      type: 'number'
    },
    {
      label: '5',
      type: 'number'
    },
    {
      label: '6',
      type: 'number'
    },
    {
      label: '*',
      type: 'operator'
    },
    {
      label: '1',
      type: 'number'
    },
    {
      label: '2',
      type: 'number'
    },
    {
      label: '3',
      type: 'number'
    },
    {
      label: '-',
      type: 'operator'
    },
    {
      label: '0',
      type: 'number'
    },
    {
      label: '.',
      type: 'number'
    },
    {
      label: '=',
      type: 'return'
    },
    {
      label: '+',
      type: 'operator'
    }
  ],
    
  methods: {
    setInputValue (value: string){
        this.$set(this, 'inputValue', value)
      },
    setLabelValue(value: string){
      this.$set(this, 'labelValue', value)
      },
    validateInputVal(event: KeyboardEvent){
      const allowedSymbols = '0123456789+-*/%().'
      event.preventDefault()
      // if(!allowedSymbols.includes(event.key) &&  event.key!=='Enter'){
      //   return event.preventDefault()
      // } else 
      if (event.key==='Enter'){
        
        this.onCalcInput('=')
      } else if (allowedSymbols.includes(event.key)){
        console.log('HELLOOOO', event)
        this.onCalcInput(event.key)
      }
      
      // const lastTypedChar = event.slice(-1);
      // onCalcInput(lastTypedChar);
    },
    onCalcInput(buttonLabel: string) {
      console.log('onCalcInput', buttonLabel)

      const inputNumbers='0123456789';
      const inputEvalOperators = '+-*/%().';
      const noDotAfterChars = '+-*/%().';
      const lastChar = this.inputValue.slice(-1)
      const numbersAr = this.inputValue.match(/\d*\.?\d*/gm);
      const lastNumberFilter = numbersAr ? numbersAr.filter(string=>string).pop() : '';
      const lastNumber = lastNumberFilter ? lastNumberFilter : '';
      console.log('lastNumber',lastNumber)

      if(inputNumbers.includes(buttonLabel)){
        this.setInputValue(this.inputValue+buttonLabel)
      } 
      else if(buttonLabel==='.'){
        if(noDotAfterChars.includes(lastChar) || lastNumber.includes('.')){
          return
        }
        else{
          this.setInputValue(this.inputValue+buttonLabel);
        }
      }
      else if(inputEvalOperators.includes(buttonLabel)) {
        if(buttonLabel===lastChar){
          const newInputValue = this.inputValue.slice(0, -1)+buttonLabel;
          this.setInputValue(newInputValue)
        } else {
          this.setInputValue(this.inputValue+buttonLabel)
        }
      } 
      else if(buttonLabel==='C'){
        this.setInputValue('')
        this.setLabelValue('')
      }
      else if(buttonLabel==='='){
        const newInputValue = eval(this.inputValue).toString();
        console.log('newInputValue', newInputValue)
        if ( newInputValue !== 'undefined' && typeof newInputValue === 'string'){
          this.setLabelValue(this.inputValue+'=')
          this.setInputValue(newInputValue);
          
        }
        

      }
      console.log(buttonLabel)
      
    }
  }
  
});

</script>

<template>
  <q-page class="row items-center justify-evenly">
    <div class="calculatorWrapper">
      <q-input 
        outlined 
        v-model="inputValue"
        v-on:keypress="validateInputVal($event)"
        :label="labelValue" 
        class="mainCalcInput"
      />
      <div  class="calcButtons">
        <ButtonComponent 
        v-for="button in $options.calcButtons" 
        :key="button.label" 
        :onClick="onCalcInput" 
        :label="button.label" 
        class="calcButton"
        :type="button.type"/>
      </div>
    </div>
  </q-page>
</template>

<style>
.calculatorWrapper{
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}
.calcButtons{
  max-width: 382px;
  display: flex;
  flex-direction: row;
  flex-wrap: wrap;
  justify-content: center
}
.calcButton{
  width: 83px;
  height: 34px;
  margin: 5px;
}
.mainCalcInput{
  width: 362px;
  margin-bottom: 5px;
}
.mainCalcInput input{ 
    text-align: right; 
}
.mainCalcInput .q-field__label{
  text-align: right; 
}
</style>