<template>
  <!--Esencialmente, quise ver tutoriales para hacer la calculadora, pero todo lo que encontré
  eran videos en los que simplemente programaban la calculadora sin explicar el como, por lo que terminé
  con un resultado demasiado similar, de cualquier forma añadí de mi parte, cambié el estilo por algo más
  agradable, corregí errores de funcionamiento que tenía el código original, añadí cosas nuevas respecto al
  original.
  También intenté añdadir un historial, pero no jaló y me quedo sin tiempo X'D-->
  <!--Añadí otro container para centrar verticalmente la calculadora-->
  <div class="calculatorContainer">
    <div class="calculator">
      <div class="display">{{current || '0'}}</div>
      <div @click="clear" class="btn">C</div>
      <div @click="sign" class="btn">+/-</div>
      <div @click="percent" class="btn">%</div>
      <div @click="divideNumbers" class="btn operatorButtons">÷</div>
      <div @click="append('7')" class="btn">7</div>
      <div @click="append('8')" class="btn">8</div>
      <div @click="append('9')" class="btn">9</div>
      <div @click="timesNumbers" class="btn operatorButtons">x</div>
      <div @click="append('4')" class="btn">4</div>
      <div @click="append('5')" class="btn">5</div>
      <div @click="append('6')" class="btn">6</div>
      <div @click="minusNumbers" class="btn operatorButtons">-</div>
      <div @click="append('1')" class="btn">1</div>
      <div @click="append('2')" class="btn">2</div>
      <div @click="append('3')" class="btn">3</div>
      <div @click="addNumbers" class="btn operatorButtons">+</div>
      <div @click="append('0')" class="btn zero">0</div>
      <div @click="dot" class="btn">.</div>
      <div @click="equalResult" class="btn operatorButtons">=</div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      //El código original solo era capaz de hacer una operación a la vez
      //le añadí un historial para que pueda realizar cadenas de operaciones
      //Se explica más adelante
      history: [], 
      current: '',
      operatorClicked: false,
      operatorButtons: null,
    }
  },
  methods: {
    clear() {
      this.current = '';
      this.history = [];
    },
    sign() {
      this.current = this.current.charAt(0) === '-' ?
        this.current.slice(1) : `-${this.current}`;
    },
    percent() {
      this.current = `${parseFloat(this.current) / 100}`;
    },
    //En el código original se concatenaba el número a this.current, ahora se añade al historial.
    append(number) {
      if (this.operatorClicked) {
        this.operatorClicked = false;
      }
      this.current += number;
    },
    dot() {
      if (this.current.indexOf('.') === -1) {
        this.append('.');
      }
    },
    //Se agrega la operación y el número al historial
    setPrevious(operator) {
      this.history.push(parseFloat(this.current), operator);
      this.current = '';
      this.operatorClicked = true;
    },
    divideNumbers() {
      this.setPrevious('/');
    },
    timesNumbers() {
      this.setPrevious('*');
    },
    minusNumbers() {
      this.setPrevious('-');
    },
    addNumbers() {
      this.setPrevious('+');
    },
    //Ahora equalResult evalúa el historial
    equalResult() {
      this.history.push(parseFloat(this.current));
      let result = this.history[0];

      for (let i = 1; i < this.history.length; i += 2) {
        const operator = this.history[i];
        const operand = this.history[i + 1];

        if (operator === '+') {
          result += operand;
        } else if (operator === '-') {
          result -= operand;
        } else if (operator === '*') {
          result *= operand;
        } else if (operator === '/') {
          result /= operand;
        }
      }

      this.current = result.toString();
      this.history = [];
    },
  },
}
</script>

<style scoped>
.calculatorContainer {
  background-color: rgb(21, 3, 73);
  display: flex;
  align-items: center;
  justify-content: center;
  height: 100vh;
}

.calculator {
  margin: 0 auto;
  padding: 0 auto;
  width: 400px;
  font-size: 40px;
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  grid-auto-rows: minmax(50px, auto);
}

.display {
  grid-column: 1 / 5;
  background-color: #1d1d1d;
  color: white;
  border: 2px solid white;
  border-radius: 5px;
}

.zero {
  grid-column: 1 / 3;
}

.btn {
  background-color: #000000;
  border: 2px solid #ffffff;
  border-radius: 5px;
  color: white;
}

.btn:hover{
  border: 2px solid black;
  background-color: #3ac246;
}

.operatorButtons {
  background-color: rgb(0, 0, 0);
  color: white;
}

.operatorButtons:hover{
  background-color: rgb(26, 170, 196);
}
</style>
