<template>
  <div>
    <h1 class="title has-text-centered">Welcome to Vue-Doughtnut-Chart</h1>

    <div class="columns">
      <div class="column">
        <DoughnutChart :percent="percent"/>
      </div>
      <div class="column">
        <DoughnutChart :percent="percent" foregroundColor="green"/>
      </div>
      <div class="column">
        <DoughnutChart :percent="percent" foregroundColor="red"/>
      </div>
      <div class="column">
        <DoughnutChart :percent="percent" foregroundColor="purple"/>
      </div>
    </div>
    <input type="range" min="0" step="1" max="100" v-model="percent"> {{percent}}% <br>
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
      percent: 25
    }
  }
}
</script>

<style lang="sass" scoped>

</style>

