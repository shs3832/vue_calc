<template>
  <div>
    <nav class="navigation">
      <div class="row">
        <button type="button" @click="visible = true" class="btn">
          마통이자계산
        </button>
        <button type="button" @click="visible = false" class="btn">
          통장이자계산
        </button>
      </div>
    </nav>
    <div class="container-md" v-if="visible">
      <minus @minus="minusValue"></minus>
    </div>
    <div class="container-md" v-else>
      <toss @toss="tossValue"></toss>
    </div>
    <div class="fixedResult">
      <div>
        마통이자계산결과<br />
        {{ value1 }}
      </div>
      <div>
        통장이자계산결과<br />
        {{ value2 }}
      </div>
    </div>
  </div>
</template>

<script>
import Minus from "./page/minus.vue";
import Toss from "./page/toss.vue";
export default {
  name: "App",

  components: {
    Toss,
    Minus,
  },
  data() {
    return {
      visible: true,
      value1: "0",
      value2: "0",
    };
  },
  methods: {
    minusValue(val) {
      this.value1 = val;
    },
    tossValue(val) {
      this.value2 = val;
    },
  },
};
</script>

<style>
body {
  margin: 0;
  padding: 0;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: left;
  color: #2c3e50;
}
.navigation {
  background: darkslateblue;
  padding: 10px;
  overflow: hidden;
}
.navigation button {
  width: 50%;
  color: #fff !important;
  border-radius: 0;
}
.navigation button:first-child {
  border-right: 1px solid #fff;
}
.fixedResult {
  position: relative;
  width: 100%;
  padding: 20px;
  display: flex;
  background: #ccc;
  bottom: 0;
  left: 0;
}
.fixedResult div {
  width: 50%;
  text-align: center;
}
</style>
