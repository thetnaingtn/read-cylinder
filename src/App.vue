<template>
  <div class="container">
    <el-card>
      <div class="time">
        <h1 class="time-title unicode">ကြာချိန်</h1>
        <div class="time-block">
          <span class="time-block__value">{{ result[0] }}</span>
          <span class="time-block__unit unicode">ရက်</span>
          <span class="time-block__value">{{ result[1] }}</span>
          <span class="time-block__unit unicode">နာရီ</span>
          <span class="time-block__value unicode">{{ result[2] }}</span>
          <span class="time-block__unit unicode">မိနစ်</span>
        </div>
      </div>
    </el-card>
    <el-select v-model="psiValue" placeholder="Select PSI">
      <el-option v-for="psi in psis" :key="psi" :label="psi" :value="psi">
      </el-option>
    </el-select>
    <el-select v-model="literValue" placeholder="Select Cylinder Size(L)">
      <el-option
        v-for="liter in liters"
        :key="liter"
        :label="liter"
        :value="liter"
      />
    </el-select>
    <el-select v-model="flowValue" placeholder="Select Flow Rate(L/min)">
      <el-option
        v-for="flowRate in flowRates"
        :key="flowRate"
        :label="flowRate"
        :value="flowRate"
      />
    </el-select>
  </div>
</template>

<script>
import { ref } from "vue";
import unit from "./unit.json";
import availableData from "./data.json";
export default {
  setup() {
    let data = availableData;
    let unitData = JSON.parse(JSON.stringify(unit));
    let psis = ref(unitData.psi);
    let liters = ref(unitData.liter);
    let flowRates = ref(unitData.flowRate);
    let psi = ref(null);
    let liter = ref(null);
    let flow = ref(null);
    return {
      data,
      psis,
      psi,
      liters,
      liter,
      flowRates,
      flow,
    };
  },
  computed: {
    psiValue: {
      get() {
        return this.psi ? `${this.psi} psi` : this.psi;
      },
      set(val) {
        this.psi = val;
      },
    },
    literValue: {
      get() {
        return this.liter ? `${this.liter} L` : this.liter;
      },
      set(val) {
        this.liter = val;
      },
    },
    flowValue: {
      get() {
        return typeof this.flow === "number" ? `${this.flow} L/min` : this.flow;
      },
      set(val) {
        this.flow = val;
      },
    },
    result() {
      if (!this.psiValue || !this.literValue || !this.flowValue)
        return "00:00:00".split(":");
      let str =
        this.data[this.psiValue][this.literValue][this.flow - 1]?.split(":");
      if (str.length < 3) str = ["00", ...str];
      return str;
    },
  },
};
</script>

<style>
@font-face {
  font-family: "Pyidaungsu";
  src: url("./assets/pyidaungsu.ttf");
}
:root {
  --color-grey-1: #f4f4f4;
  --color-grey-2: #d1d1d1;
  --color-grey-3: #474747;
}
.unicode {
  font-family: "Pyidaungsu";
}
.container {
  font-family: "Roboto", sans-serif;
  margin: 1.25em auto;
  max-width: 25em;
  width: 100%;
  display: flex;
  flex-direction: column;
}

.container > * + * {
  margin-top: 2.5em;
}

.time {
  flex: 0 0 40%;
  display: flex;
  justify-content: space-between;
  font-size: 1.5625rem;
  align-items: center;
}

.time-title {
  font-size: inherit;
}

.time-block {
  display: flex;
  align-items: center;
  justify-content: space-between;
}

.time-block > .time-block__value {
  border-radius: 0.16em;
  padding: 0.24em;
  margin: 0 0.16em;
  border: 1px solid var(--color-grey-1);
}

.time-block__unit {
  font-size: 0.9375rem;
  color: var(--color-grey-2);
}

.time-title,
.time-block__value {
  color: var(--color-grey-3);
}
</style>