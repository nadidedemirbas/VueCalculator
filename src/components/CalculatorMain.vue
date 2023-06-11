<template>
  <div class="main-container">
    <div class="monitor">
      <span class="operation-showcase">{{ smallText }}</span>
      <span class="answer">{{ largeText }}</span>
    </div>
    <div class="button-row">
      <Button :call="clearAll" :parameter="''" :text="'C'" />
      <Button :call="backSpace" :parameter="''" :text="'<'" />
      <Button :call="setOperation" :parameter="'percentage'" :text="'%'" />
      <Button :call="setOperation" :parameter="'divide'" :text="'/'" :className="'orange'" />
    </div>
    <div class="button-row">
      <Button :call="addDigit" :parameter="'7'" :text="'7'" />
      <Button :call="addDigit" :parameter="'8'" :text="'8'" />
      <Button :call="addDigit" :parameter="'9'" :text="'9'" />
      <Button :call="setOperation" :parameter="'multiply'" :text="'x'" :className="'orange'" />
    </div>
    <div class="button-row">
      <Button :call="addDigit" :parameter="'4'" :text="'4'" />
      <Button :call="addDigit" :parameter="'5'" :text="'5'" />
      <Button :call="addDigit" :parameter="'6'" :text="'6'" />
      <Button :call="setOperation" :parameter="'subtract'" :text="'-'" :className="'orange'" />
    </div>
    <div class="button-row">
      <Button :call="addDigit" :parameter="'1'" :text="'1'" />
      <Button :call="addDigit" :parameter="'2'" :text="'2'" />
      <Button :call="addDigit" :parameter="'3'" :text="'3'" />
      <Button :call="setOperation" :parameter="'add'" :text="'+'" :className="'orange'" />
    </div>
    <div class="button-row">
      <Button :call="addDigit" :parameter="'0'" :text="'0'" :className="'wide'"/>
      <Button :call="addDigit" :parameter="'.'" :text="','" />
      <Button :call="operate" :parameter="''" :text="'='" :className="'orange'" />
    </div>
  </div>
</template>

<script>
import Button from "./Button.vue";
export default {
  name: "CalculatorMain",
  components: {Button},
  data() {
    return {
      smallText: '',
      largeText: '0',
      firstNum: '',
      firstNumFinal: '',
      firstNumSet: false,
      secondNum: '',
      operation: '',
      operatorSign: '',
      answer: '',
      answerSet: false
    }
  },
  methods: {
    addDigit(newDigit) {
      if(this.answerSet) {
        this.clearAll();
      }
      if (this.firstNumSet) {
        this.secondNum  = String(this.secondNum) + String(newDigit);
        this.smallText = String(this.firstNumFinal) + ' ' + this.operatorSign + ' ' + this.secondNum;
        return;
      }
      this.firstNum  = String(this.firstNum) + String(newDigit);
      this.smallText = String(this.firstNum);
    },
    setFirstNum() {
      this.firstNumSet = true;
      if (this.answerSet) {
        this.firstNumFinal = this.answer;
        this.answerSet = false;
        this.secondNum = '';
      } else {
        this.firstNumFinal = this.firstNum;
      }
    },
    add () {
      return parseFloat(this.firstNumFinal) + parseFloat(this.secondNum);
    },
    subtract () {
      return parseFloat(this.firstNumFinal) - parseFloat(this.secondNum);
    },
    multiply () {
      return parseFloat(this.firstNumFinal) * parseFloat(this.secondNum);
    },
    divide () {
      return parseFloat(this.firstNumFinal) / parseFloat(this.secondNum);
    },
    percent () {
      return (100.0 * this.firstNumFinal) / this.secondNum;
    },
    setOperation(operation) {
      this.setFirstNum();
      this.secondNum = '';
      this.operation = operation;
      switch (this.operation) {
        case 'add':
          this.operatorSign = '+';
          break;
        case "subtract":
          this.operatorSign = '-';
          break;
        case "multiply":
          this.operatorSign = 'x';
          break;
        case "divide":
          this.operatorSign = '/';
          break;
        case "percentage":
          this.operatorSign = '%';
          break;
      }
      this.smallText = String(this.firstNumFinal) + ' ' + this.operatorSign;
    },
    operate() {
      let answer = 0;
      switch (this.operation) {
        case 'add':
          answer = this.add();
          break;
        case "subtract":
          answer = this.subtract();
          break;
        case "multiply":
          answer = this.multiply();
          break;
        case "divide":
          answer = this.divide();
          break;
        case "percentage":
          answer = this.percent();
          break;
      }
      answer = parseFloat(answer.toFixed(5))
      if (answer.isNaN) return;
      this.answer = answer;
      this.answerSet = true;
    },
    clearAll () {
      this.smallText = '';
      this.largeText = '0';
      this.firstNum = '';
      this.firstNumFinal = '';
      this.secondNum = '';
      this.firstNumSet = false;
      this.answerSet = false;
    },
    backSpace () {
      if (this.firstNumSet) {
        this.secondNum = this.secondNum.slice(0, -1);
      } else {
        this.firstNum = this.firstNum.slice(0, -1);
      }
      this.addDigit('');
    }
  },
  watch: {
    answer(newAnswer) {
      this.largeText = newAnswer;
    }
  }
}

</script>

<style>
@import '/css/style.css';
</style>