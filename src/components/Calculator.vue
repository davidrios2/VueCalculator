<template>
  <div>
    <div style="display:flex">
      <div class="calculator">
        <div class="display">{{current || '0'}}</div>
        <div @click="clear" class="btn operator">C</div>
        <div @click="append('(')" class="btn operator">(</div>
        <div @click="append(')')" class="btn operator">)</div>
        <div @click="append('/')" class="btn operator">/</div>
        <div @click="append('7')" class="btn">7</div>
        <div @click="append('8')" class="btn">8</div>
        <div @click="append('9')" class="btn">9</div>
        <div @click="append('*')" class="btn operator">*</div>
        <div @click="append('4')" class="btn">4</div>
        <div @click="append('5')" class="btn">5</div>
        <div @click="append('6')" class="btn">6</div>
        <div @click="append('-')" class="btn operator">-</div>
        <div @click="append('1')" class="btn">1</div>
        <div @click="append('2')" class="btn">2</div>
        <div @click="append('3')" class="btn">3</div>
        <div @click="append('+')" class="btn operator">+</div>
        <div @click="append('0')" class="btn">0</div>
        <div @click="append('.')" class="btn">.</div>
        <div @click="equal" class="btn operator">=</div>
        <div @click="append('**')" class="btn operator">**</div>
      </div>
      <div class="tokens">
        <ul>
            <li v-for="tuple in tokens">
              {{tuple.TOKEN}}: {{tuple.TIPO}}
            </li>
        </ul>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios';
export default {
  data() {
    return {
      previous: null,
      current: '',
      tokens: '',
      operator: null,
    }
  },
  methods: {
    clear() {
      this.current = '';
    },
    append(number) {
      this.current = `${this.current}${number}`;
    },

    equal() {
      axios.post('https://lexcalculator-1-o8855159.deta.app/tokenizerSolverCalculator', {
        expression: this.current
      }, {
        headers: {
          'Content-Type': 'application/json'
        }
      })
          .then((response) => {
            console.log(response);
            this.tokens = response.data.TOKENS
            this.current = response.data.RESULTADO
          }, (error) => {
            console.log(error);
          });
    }
  }
}
</script>

<style scoped>
.calculator {
  margin: 0 auto;
  width: 700px;
  font-size: 40px;
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  grid-auto-rows: minmax(50px, auto);
}
.display {
  grid-column: 1 / 5;
  background-color: #333;
  color: white;
}
.btn {
  background-color: #F2F2F2;
  border: 1px solid #999;
}
.operator {
  background-color: orange;
  color: white;
}
.tokens {
  color: #F2F2F2;
  font-size: 18px;
  width: 30%;
}
li::marker {
  content: '';
}
</style>