# 🍩 Vue Doughnut Chart

> Doughnut chart component for Vue.js, originally created by Greg Willson in [codepen](https://codepen.io/biomassives/pen/yaZwQw)

[![version](https://img.shields.io/npm/v/vue-doughnut-chart.svg)](https://www.npmjs.com/package/vue-doughnut-chart) [![downloads](https://img.shields.io/npm/dt/vue-doughnut-chart.svg)](https://www.npmjs.com/package/vue-doughnut-chart) [![Travis](https://img.shields.io/travis/mazipan/vue-doughnut-chart.svg)](https://travis-ci.org/mazipan/vue-doughnut-chart)

## Demo

[https://mazipan.github.io/vue-doughnut-chart](https://mazipan.github.io/vue-doughnut-chart)

## Install

```shell
yarn add vue-doughnut-chart
# OR
npm i vue-doughnut-chart
```

## Use in components

```js
import DoughnutChart from 'vue-doughnut-chart'

export default {
  components: {
    DoughnutChart
  }
}
```

## Props

| Props Name          | Type      | Default Value |Description                   | 
|---------------------|-----------|---------------|------------------------------|
| `percent`           | Number    |  0            |                              |
| `foregroundColor`   | String    |  '#1993ff'    |                              |
| `backgroundColor`   | String    |  '#ecf6ff'    |                              |
| `strokeWidth`       | Number    |  10           |                              |
| `radius`            | Number    |  85           |                              |
| `width`             | Number    |  200          |                              |
| `height`            | Number    |  200          |                              |
| `visibleValue`      | Boolean   |  false        |                              |
| `emptyText`         | String    |  ''           |                              |
| `classValue`        | String    |  ''           |                              |

-----

Bringing to NPM Registry by Irfan Maulana © 2018
