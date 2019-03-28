<template>
  <div>
    <div class="mode">
      Advanced mode
      <toggle-button v-model="advancedMode"/>
    </div>
    <div class="calculator" :class="{'advanced' : advancedMode}">
      <div class="display">{{current || '0'}}</div>
      <button class="btn first"    @click="clear">AC</button>
      <button class="btn first"    @click="sign">±</button>
      <button class="btn first"    @click="percent">%</button>
      <button class="btn operator" @click="divide">÷</button>
      <button class="btn func"     @click="sin">sin</button>
      <button class="btn func"     @click="sinh">sin⁻¹</button>
      <button class="btn"          @click="append(7)">7</button>
      <button class="btn"          @click="append(8)">8</button>
      <button class="btn"          @click="append(9)">9</button>
      <button class="btn operator" @click="times">×</button>
      <button class="btn func"     @click="cos">cos</button>
      <button class="btn func"     @click="cosh">cos⁻¹</button>
      <button class="btn"          @click="append(4)">4</button>
      <button class="btn"          @click="append(5)" >5</button>
      <button class="btn"          @click="append(6)" >6</button>
      <button class="btn operator" @click="minus">-</button>
      <button class="btn func"     @click="tan">tan</button>
      <button class="btn func"     @click="tanh">tan⁻¹</button>
      <button class="btn"          @click="append(1)">1</button>
      <button class="btn"          @click="append(2)">2</button>
      <button class="btn"          @click="append(3)">3</button>
      <button class="btn operator" @click="add" >+</button>
      <button class="btn func"     @click="sqrt">√</button>
      <button class="btn func"     @click="log">log₂</button>
      <button class="btn zero"     @click="append(0)">0</button>
      <button class="btn"          @click="dot">.</button>
      <button class="btn operator" @click="equals">=</button>
      <button class="btn func"     @click="pow(2)">x²</button>
      <button class="btn func"     @click="pow(-1)">x⁻¹ </button>
    </div>
  </div>
</template>

<script>
import { ToggleButton } from 'vue-js-toggle-button'

export default {
  name : "calculator", 
  data() {
    return {
      current : '',
      previous: null,
      operator: null,
      operatorSet : false,
      advancedMode : false
    }
  },
  components : {
    ToggleButton
  },
  methods : {
    clear() {
      this.current = '';
      this.previous = null;
      this.operator = null;
      this.operatorSet = false;
    },
    sign() {
      this.current = `${parseFloat(this.current || '0') * -1}`;
    },
    percent() {
      this.current = `${parseFloat(this.current || '0') / 100}`;
    },
    append(nr) {
      if (this.current == '0' || this.operatorSet) {
        this.current = '';
        this.operatorSet = false;
      }

      this.current = `${this.current}${nr}`;
    },
    dot() {
      if (this.current.indexOf('.') === -1)
        this.append('.')
    },
    setPrevious() {
      if (this.previous != null)
        this.equals();

      this.previous = this.current;
      this.operatorSet = true;
    },
    minus() {
      this.setPrevious();
      this.operator = (a, b) => a - b;
    },
    add() {
      this.setPrevious();
      this.operator = (a, b) => a + b;
    },
    divide(){
      this.setPrevious();
      this.operator = (a, b) => a / b;
    },
    times() {
      this.setPrevious();
      this.operator = (a, b) => a * b;
    },
    sin() {
      this.current = `${Math.sin(parseFloat(this.current || '0'))}`;
    },
    cos() {
      this.current = `${Math.cos(parseFloat(this.current || '0'))}`;
    },
    tan() {
      this.current = `${Math.tan(parseFloat(this.current || '0'))}`;
    },
    sinh() {
      this.current = `${Math.sinh(parseFloat(this.current || '0'))}`;
    },
    cosh() {
      this.current = `${Math.cosh(parseFloat(this.current || '0'))}`;
    },
    tanh() {
      this.current = `${Math.tanh(parseFloat(this.current || '0'))}`;
    },
    sqrt() {
      this.current = `${Math.sqrt(parseFloat(this.current || '0'))}`;
    },
    log() {
      this.current = `${Math.log2(parseFloat(this.current || '0'))}`;      
    },
    pow(exp) {
      this.current = `${Math.pow(parseFloat(this.current || '0'), exp)}`;
    },
    equals() {
      if (this.operator !== null) {
        this.current = `${this.operator(
          parseFloat(this.current), 
          parseFloat(this.previous)
        )}`;

        this.operator = null;
        this.previous = null;
      }
    }
  }
}
</script>

<style scoped>
  * {
    color: #2c3e50;
  }

  @keyframes createBox {
    from {
      transform: scale(0);
    }
    to {
      transform: scale(1);
    }
  }

  .mode {
    /* background: #111; */
    color: #f2f2f2;
    padding: 10px 0px;
    width: 200px;
    margin: 0 auto;
  }

  .calculator {
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    grid-auto-rows: minmax(50px, auto);
    font-size:30px;
    width: 300px;
    animation: createBox 2s;
    margin: 0 auto;
    transition: width 1s ease-in-out;
  }

  .calculator.advanced {
    grid-template-columns: repeat(6, 1fr);
    width: 600px;
  }

  .calculator > * {
    padding: 10px;
  }

  .display {
    grid-column: 1 / 5;
    background: #333;
    color: #fff;
  }

  .calculator.advanced .display {
    grid-column: 1 / 7;
  }

  .btn {
    background-color: #f2f2f2;
    border: 1px solid rgb(216, 216, 216) ;  
    -webkit-user-select: none; /* Safari */        
    -moz-user-select: none; /* Firefox */
    -ms-user-select: none; /* IE10+/Edge */
    user-select: none; /* Standard */
    font-size:20px;
  }

  .btn:active {
    background-color: #2c3e50;
    color: #f2f2f2;
    border: none;
  }

  .btn.first {
    background-color: #e2e2e2;
    border: 1px solid rgb(189, 189, 189) ;  
  }

  .btn.first:active {
    background-color: #2c3e50;
    color: #f2f2f2;
  }

  .zero {
    grid-column: 1 / 3;
  }

  .operator {
    background: rgb(255, 145, 0);
    color: #f2f2f2;
    border: 1px solid rgb(255, 187, 98);
  }

  .operator:active {
    background: rgb(197, 112, 0);
    color: #f2f2f2;
    border: rgb(197, 112, 0);
  }

  .func {
    display: none;
    background-color: #444;
    color: #f2f2f2;
    border: 1px solid #555;
  }

  .calculator.advanced .func {
    display: grid;
  }

  .func:active {
    background-color: #f2f2f2;
    color: #444;
  }
</style>

