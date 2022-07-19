<template>
  <div class="calculator">
    <h1>计算器</h1>
    <div class="input">
      <div class="first">{{ firstLabel }}</div>
      <div class="second">{{ secondNumber }}</div>
    </div>
    <div class="input-button">
      <div class="row">
        <button class="button icon-font-30" @click="clickClear">C</button>
        <button class="button" @click="clickDelete">
          <img :src="require('./assets/delete.svg')" height="30" width="30">
        </button>
      </div>
      <div class="row">
        <button class="button icon-font-30" @click="clickNumber">7</button>
        <button class="button icon-font-30" @click="clickNumber">8</button>
        <button class="button icon-font-30" @click="clickNumber">9</button>
        <button class="button icon-font-30" @click="clickSymbol">/</button>
      </div>
      <div class="row">
        <button class="button icon-font-30" @click="clickNumber">4</button>
        <button class="button icon-font-30" @click="clickNumber">5</button>
        <button class="button icon-font-30" @click="clickNumber">6</button>
        <button class="button icon-font-30" @click="clickSymbol">*</button>
      </div>
      <div class="row">
        <button class="button icon-font-30" @click="clickNumber">1</button>
        <button class="button icon-font-30" @click="clickNumber">2</button>
        <button class="button icon-font-30" @click="clickNumber">3</button>
        <button class="button icon-font-30" @click="clickSymbol">-</button>
      </div>
      <div class="row">
        <button class="button icon-font-30" @click="clickChange">
          <img :src="require('./assets/plus-minus.svg')" height="30" width="30">
        </button>
        <button class="button icon-font-30" @click="clickNumber">0</button>
        <button class="button icon-font-30" @click="clickNumber">.</button>
        <button class="button icon-font-30" @click="clickSymbol">+</button>
      </div>
      <div class="row">
        <button class="button icon-font-30" @click="clickEqual">=</button>
      </div>
    </div>
  </div>
</template>

<script>

export default {
  data() {
    return {
      firstNumber: "",
      secondNumber: "0",
      firstLabel: "",
      symbol: "",
    }
  },
  methods: {
    clickNumber(event) {
      this.secondNumber = this.secondNumber === "0" ? event.target.textContent : this.secondNumber + event.target.textContent;
    },
    clickSymbol(event) {
      if (this.firstLabel !== "") {
        this.clickEqual(event);
      }
      if (this.secondNumber === "0") {
        this.secondNumber = event.target.textContent;
      } else {
        this.symbol = event.target.textContent;
        this.firstLabel = this.secondNumber + this.symbol;
        this.firstNumber = this.secondNumber;
        this.secondNumber = "";
      }
    },
    clickClear() {
      this.secondNumber = "0";
      this.firstLabel = "";
      this.firstNumber = "";
    },
    clickDelete() {
      const len = this.secondNumber.length;
      if (len === 1) {
        this.secondNumber = "0";
      } else {
        this.secondNumber = (this.secondNumber + "").substring(0, len - 1);
      }
    },
    clickEqual(event) {
      this.firstLabel = this.firstLabel + this.secondNumber + event.target.textContent;
      switch (this.symbol) {
        case "+":
          this.secondNumber = parseInt(this.firstNumber) + parseInt(this.secondNumber) + "";
          break;
        case "-":
          this.secondNumber = parseInt(this.firstNumber) - parseInt(this.secondNumber) + "";
          break;
        case "*":
          this.secondNumber = this.getRes(this.secondNumber * this.firstNumber, 9);
          break;
        case "/":
          this.secondNumber = this.getRes(this.firstNumber / this.secondNumber, 9);
          break;
        default:
          break;
      }
    },
    clickChange() {
      this.secondNumber = 0 - this.secondNumber;
    },
    getRes(num, digit = 3) {
      if (Number(num) === Math.floor(num)) return Math.floor(num);
      let res = [], addNum = 0;
      num += '';
      let [zs, xs] = num.split('.');
      for (let i = 0; i < xs.length; i++) {
        const ele = xs[i];
        if (ele != 0) {
          if (xs[i + digit] && xs[i + digit] > 4) {
            addNum = 1;
            for (let j = i + (digit - 1); j >= 0; j--) {
              if (Number(xs[j]) + addNum == 10) res[j] = '0';
              else {
                res[j] = Number(xs[j]) + addNum;
                addNum = 0;
              }
            }
          } else {
            for (let k = i; k <= i + (digit - 1); k++) {
              res[k] = xs[k];
            }
          }
          break;
        } else res[i] = xs[i];
      }
      return Number(Number(zs) + addNum + '.' + res.join(''));
    }
  }
}
</script>

<style lang="scss">

#app {
  height: 100%;
}

.icon-font-30 {
  font-size: 30px;
}

.calculator {
  background-color: #efefef;
  width: 100%;
  height: 100%;
  text-align: center;
  display: flex;
  flex-direction: column;
  position: relative;

  .input {
    flex: 1;
    display: flex;
    flex-direction: column;

    .first {
      flex: 1;
      align-items: center;
      text-align: right;
      font-size: 20px;
    }

    .second {
      flex: 2;
      align-items: center;
      text-align: right;
      font-size: 50px;
    }
  }

  .input-button {
    flex: 3;
    display: flex;
    flex-direction: column;

    .row {
      flex: 1;
      display: flex;
    }

    .button {
      fleX: 1;
    }
  }

}
</style>
