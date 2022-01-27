<template>
  <div>
    <div class="container-md">
      <h1 class="mt-3">마이너스통장 일별이자계산</h1>
      <hr />
      <div class="mb-3">
        <label for="input1" class="form-label">대출금 <sub>(원)</sub></label>
        <input
          type="text"
          id="input1"
          class="form-control"
          v-model="amount1"
          @input="inputNumberAutoComma($event.target.value)"
        />
      </div>
      <div class="mb-3">
        <div class="d-flex justify-content-around">
          <button
            type="button"
            @click="addAmount(10000000)"
            class="btn btn-primary"
          >
            1000만원
          </button>
          <button
            type="button"
            @click="addAmount(1000000)"
            class="btn btn-primary"
          >
            100만원
          </button>
          <button
            type="button"
            @click="addAmount(100000)"
            class="btn btn-primary"
          >
            10만원
          </button>
        </div>
      </div>
      <div class="mb-3">
        <label for="input2" class="form-label">이율 <sub>(%)</sub></label>
        <input type="text" id="input2" class="form-control" v-model="amount2" />
      </div>
      <div class="mb-3">
        <label for="input3" class="form-label">대출일수 <sub>(일)</sub></label>
        <input type="text" id="input3" class="form-control" v-model="amount3" />
      </div>
      <div class="mb-3">
        <div class="d-grid gap-2">
          <button type="button" @click="calculator" class="btn btn-danger">
            계산하기
          </button>
        </div>
      </div>

      <div class="mb-3">
        <label for="input4" class="form-label">계산결과 </label>
        <input
          type="text"
          id="input4"
          class="form-control"
          readonly
          v-model="amount4"
        />
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      amount1: "0",
      amount2: "0",
      amount3: "0",
      amount4: "0",
    };
  },
  created() {
    localStorage.getItem("amount1", this.amount1) === null
      ? "0"
      : (this.amount1 = localStorage.getItem("amount1", this.amount1));
    localStorage.getItem("amount2", this.amount2) === null
      ? "0"
      : (this.amount2 = localStorage.getItem("amount2", this.amount2));
    localStorage.getItem("amount3", this.amount3) === null
      ? "0"
      : (this.amount3 = localStorage.getItem("amount3", this.amount3));
    this.calculator();
  },
  methods: {
    inputNumberAutoComma(values) {
      let obj = values;
      const comma = (obj) => {
        obj = String(obj);
        return obj.replace(/(\d)(?=(?:\d{3})+(?!\d))/g, "$1,");
      };
      const uncomma = (obj) => {
        obj = String(obj);
        return obj.replace(/[^\d]+/g, "");
      };
      this.amount1 = comma(uncomma(obj));
    },
    addAmount(str) {
      let value = String(this.amount1);
      let value2 = this.removeComma(value);
      let calc = String(parseInt(value2) + parseInt(str));
      calc = String(calc);
      this.amount1 = this.setComma(calc);
    },
    calculator() {
      localStorage.setItem("amount1", this.amount1);
      localStorage.setItem("amount2", this.amount2);
      localStorage.setItem("amount3", this.amount3);
      let value1 = this.removeComma(this.amount1);
      let result = Math.floor(
        (parseInt(value1) *
          (parseInt(this.amount2) * 0.01) *
          parseInt(this.amount3)) /
          365
      );
      this.amount4 = this.setComma(String(result)).concat("원");
    },
    removeComma(value) {
      return value.replace(/[^\d]+/g, "");
    },
    setComma(value) {
      return value.replace(/(\d)(?=(?:\d{3})+(?!\d))/g, "$1,");
    },
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: left;
  color: #2c3e50;
}
</style>
