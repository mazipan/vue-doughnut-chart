<template>
  <div>
    <div class="columns">
      <div class="column">
        <DoughnutChart
          :percent="percent"
          :visibleValue="true"/>
      </div>
      <div class="column">
        <DoughnutChart
          :percent="percent"
          :visibleValue="true"
          foregroundColor="green"
          backgroundColor="#d3fde2"
          :strokeWidth="20"/>
      </div>
      <div class="column">
        <DoughnutChart
          :percent="percent"
          :visibleValue="true"
          foregroundColor="red"
          :strokeWidth="30"/>
      </div>
      <div class="column">
        <DoughnutChart
          :percent="percent"
          :visibleValue="true"
          :foregroundColor="'purple'"
          :emptyText="'N/A'"
          />
      </div>
      <div class="column">
        <DoughnutChart
          :percent="percent"
          :visibleValue="true"
          :foregroundColor="'black'"
          :customText="'My value is over <br/> 9000'"
          />
    </div>
  </div>
    <div class="centered-div">
      <input type="range" min="0" step="1" max="100" v-model="percent"><br>
      <h3 class="subtitle">{{percent}}%</h3>
    </div>
  </div>
</template>

<script>
import DoughnutChart from './DoughnutChart.vue'

export default {
  name: 'App',
  components: {
    DoughnutChart
  },
  data() {
    return {
      percent: '25',
      text: 'Download Speed 8 Mbps',
    }
  }
}
</script>

<style lang="scss" scoped>
.centered-div{
  margin: 0 auto;
  text-align: center;
}
input[type=range] {
  -webkit-appearance: none;
  margin: 10px 0;
  width: 100%;
}
input[type=range]:focus {
  outline: none;
}
input[type=range]::-webkit-slider-runnable-track {
  width: 100%;
  height: 12.8px;
  cursor: pointer;
  animate: 0.2s;
  box-shadow: 0px 0px 0px #000000, 0px 0px 0px #0d0d0d;
  background: #ac51b5;
  border-radius: 25px;
  border: 0px solid #000101;
}
input[type=range]::-webkit-slider-thumb {
  box-shadow: 0px 0px 0px #000000, 0px 0px 0px #0d0d0d;
  border: 0px solid #000000;
  height: 20px;
  width: 39px;
  border-radius: 7px;
  background: #65001c;
  cursor: pointer;
  -webkit-appearance: none;
  margin-top: -3.6px;
}
input[type=range]:focus::-webkit-slider-runnable-track {
  background: #ac51b5;
}
input[type=range]::-moz-range-track {
  width: 100%;
  height: 12.8px;
  cursor: pointer;
  animate: 0.2s;
  box-shadow: 0px 0px 0px #000000, 0px 0px 0px #0d0d0d;
  background: #ac51b5;
  border-radius: 25px;
  border: 0px solid #000101;
}
input[type=range]::-moz-range-thumb {
  box-shadow: 0px 0px 0px #000000, 0px 0px 0px #0d0d0d;
  border: 0px solid #000000;
  height: 20px;
  width: 39px;
  border-radius: 7px;
  background: #65001c;
  cursor: pointer;
}
input[type=range]::-ms-track {
  width: 100%;
  height: 12.8px;
  cursor: pointer;
  animate: 0.2s;
  background: transparent;
  border-color: transparent;
  border-width: 39px 0;
  color: transparent;
}
input[type=range]::-ms-fill-lower {
  background: #ac51b5;
  border: 0px solid #000101;
  border-radius: 50px;
  box-shadow: 0px 0px 0px #000000, 0px 0px 0px #0d0d0d;
}
input[type=range]::-ms-fill-upper {
  background: #ac51b5;
  border: 0px solid #000101;
  border-radius: 50px;
  box-shadow: 0px 0px 0px #000000, 0px 0px 0px #0d0d0d;
}
input[type=range]::-ms-thumb {
  box-shadow: 0px 0px 0px #000000, 0px 0px 0px #0d0d0d;
  border: 0px solid #000000;
  height: 20px;
  width: 39px;
  border-radius: 7px;
  background: #65001c;
  cursor: pointer;
}
input[type=range]:focus::-ms-fill-lower {
  background: #ac51b5;
}
input[type=range]:focus::-ms-fill-upper {
  background: #ac51b5;
}
.column {
  display: flex;
  flex-flow: column nowrap;
  align-items: center;
  justify-content: center;
}
</style>

