<template>
  <div>
    <h1 class="mt-3">거래은행 일별이자계산</h1>
    <small>이자과세 15.4%</small>
    <hr />
    <div class="mb-3">
      <label for="input5" class="form-label">예치금 <sub>(원)</sub></label>
      <input
        type="text"
        id="input5"
        class="form-control"
        v-model="amount5"
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
        <button type="button" @click="resetAmount" class="btn btn-primary">
          예치금 초기화
        </button>
      </div>
    </div>
    <div class="mb-3">
      <label for="input6" class="form-label">이율 <sub>(%)</sub></label>
      <input
        type="text"
        id="input6"
        class="form-control"
        v-model="amount6"
        @input="storageSet('amount6', amount6)"
      />
    </div>
    <div class="mb-3">
      <label for="input7" class="form-label">거치일수 <sub>(일)</sub></label>
      <input
        type="text"
        id="input7"
        class="form-control"
        v-model="amount7"
        @input="storageSet('amount7', amount7)"
      />
    </div>
    <div class="mb-3">
      <div class="d-grid gap-2">
        <button type="button" @click="calculator" class="btn btn-danger">
          계산하기
        </button>
      </div>
      <div class="d-grid gap-2 mt-3">
        <button type="button" @click="resetAll" class="btn btn-warning">
          초기화
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
        v-model="amount8"
      />
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      amount5: "0",
      amount6: "3",
      amount7: "0",
      amount8: "0",
    };
  },
  created() {
    localStorage.getItem("amount5", this.amount5) === null
      ? this.amount5
      : (this.amount5 = localStorage.getItem("amount5", this.amount5));
    localStorage.getItem("amount6", this.amount6) === null
      ? this.amount6
      : (this.amount6 = localStorage.getItem("amount6", this.amount6));
    localStorage.getItem("amount7", this.amount7) === null
      ? this.amount7
      : (this.amount7 = localStorage.getItem("amount7", this.amount7));
    this.calculator();
  },
  methods: {
    storageSet(target, value) {
      localStorage.setItem(`${target}`, value);
    },
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
      this.amount5 = comma(uncomma(obj));
    },
    addAmount(str) {
      str === "" ? (str = "0") : str;
      let value = String(this.amount5);
      let value2 = this.removeComma(value);
      let calc = String(parseInt(value2) + parseInt(str));
      calc = String(calc);
      this.amount5 = this.setComma(calc);
      localStorage.setItem("amount5", this.amount5);
    },
    resetAmount() {
      this.amount5 = "0";
    },
    resetAll() {
      this.amount5 = "0";
      this.amount7 = "0";
    },
    calculator() {
      localStorage.setItem("amount5", this.amount5);
      localStorage.setItem("amount6", this.amount6);
      localStorage.setItem("amount7", this.amount7);
      let value1 = this.removeComma(this.amount5);
      let result = Math.floor(
        (parseInt(value1) * (parseInt(this.amount6) * 0.01)) / 365
      );
      let result2 = result * this.amount7;
      let result3 = Math.floor(result2 - result2 * 0.154);
      this.amount8 = this.setComma(String(result3)).concat("원");
      this.$emit("toss", this.amount8);
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

<style></style>
