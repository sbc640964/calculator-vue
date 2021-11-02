<script setup>

import RowKeys from "./RowKeys.vue";</script>

<template>
  <div class="flex flex-col shadow-lg absolute w-[400px] h-[500px] bg-gray-700 top-1/2 left-1/2 transform -translate-x-1/2 -translate-y-1/2 rounded-lg">
    <div class="px-6 pt-6 flex justify-between">
      <div class="font-bold text-gray-300 text-2xl">CASIO</div>
      <div class="w-28 h-8 rounded bg-gray-600"></div>
    </div>
    <div class="px-7 pt-6">
      <div class="bg-gray-300 w-8/10 h-20 rounded-lg ring-4 ring-gray-300 ring-opacity-40">
        <div
            class="font-calculator text-right px-2 shadow-inner overflow-hidden text-7xl h-full leading-20 w-full truncate leading-tight"
        >
          {{ currentNumber ?? result }}{{getDot}}
        </div>
      </div>
    </div>

    <div class="px-6 pt-2 pb-4 flex-grow mt-3">
      <RowKeys :keys="keys[0]" height="h-[9.09091%]" @key="keyPress"/>
      <RowKeys :keys="keys[1]" @key="keyPress"/>
      <RowKeys :keys="keys[2]" @key="keyPress"/>
      <RowKeys :keys="keys[3]" @key="keyPress"/>
      <div class="h-[36.36364%] flex w-full">
        <div class="h-full w-[80%]">
          <RowKeys :keys="keys[4]" height="h-1/2" m="l" @key="keyPress"/>
          <RowKeys :keys="keys[5]" height="h-1/2" m="l" @key="keyPress"/>
        </div>
        <div class="h-full w-[20%]">
          <RowKeys :keys="keys[6]" height="h-full" m="r" @key="keyPress"/>
        </div>
      </div>
    </div>
  </div>
</template>

<script>

export default {
  data: () => {
    return {
      keys: [
          ['M/EX', '√', '%', 'TAX', 'TAX+'],
          ['+/_', 'MRC', 'M-', 'M+', '÷'],
          ['>', '7', '8', '9', 'X'],
          ['C', '4', '5', '6', '-'],
          ['AC', '1', '2', '3'],
          ['0', '00', '.', '='],
          ['+']
      ],

      result: null,

      currentOperator: null,

      statusNewScreen: false,

      currentNumber: '0',
    }
  },
  computed: {
    getDot(){
      return this.currentNumber.indexOf('.') >= 0 ? '' : '.';
    }
  },
  methods:{
    keyPress(key)
    {
      key = String(key);

      if(['0','1','2','3','4','5','6','7','8','9','00', '.'].includes(key)) {
        if(this.statusNewScreen){
          this.currentNumber = '0';
        }
        const temp = this.currentNumber + key;
        if (temp.match(/\./g)?.length > 1) return;
        this.statusNewScreen = false;
        return this.currentNumber = temp.indexOf('.') === temp.length - 1 ? temp : String(Number(temp))
      }

      switch (key) {
        case 'AC':
        case 'C' :
          this.reset();
          break;
        case '>' :
          this.currentNumber = this.currentNumber.slice(0, - 1);
          if(!this.currentNumber) this.currentNumber = '0';
          break;
        case 'X':
        case '÷':
        case '-':
        case '+':
          if(this.statusNewScreen){
            return this.currentOperator = key;
          }
          this.result = this.result && this.currentOperator
              ? this.getResult(this.result, this.currentOperator, this.currentNumber)
              : this.result = this.currentNumber;
          this.currentOperator = key;
          this.statusNewScreen = true;
          break;
        case '=' :
          if(this.result && this.currentOperator) {
            this.currentNumber = String(
                this.result =
                    this.getResult(this.result, this.currentOperator, this.currentNumber)
            )
            this.currentOperator = null;
            this.statusNewScreen = true;
          }
      }
    },

    getResult(num, operator, numb){
      let result;
      switch (operator){
        case 'X' : result = Number(num) * Number(numb); break;
        case '÷' : result = Number(num) / Number(numb); break;
        case '+' : result = Number(num) + Number(numb); break;
        case '-' : result = Number(num) - Number(numb); break;
      }
      this.currentNumber = String(result);
      return result;
    },

    reset(){
      this.currentNumber = '0';
      this.statusNewScreen = false;
      this.result = null;
      this.currentOperator = null;
    }
  }
}
</script>

<style scoped>
  .key>div{
    @apply rounded flex justify-center items-center bg-gray-800;
  }
</style>
