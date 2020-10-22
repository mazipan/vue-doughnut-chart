# 🍩 Vue Doughnut Chart

> Doughnut chart component for Vue.js, originally created by Greg Willson in [codepen](https://codepen.io/biomassives/pen/yaZwQw)

[![version](https://img.shields.io/npm/v/vue-doughnut-chart.svg)](https://www.npmjs.com/package/vue-doughnut-chart) ![minified](https://badgen.net/bundlephobia/minzip/vue-doughnut-chart) [![downloads](https://img.shields.io/npm/dt/vue-doughnut-chart.svg)](https://www.npmjs.com/package/vue-doughnut-chart) [![Travis](https://img.shields.io/travis/mazipan/vue-doughnut-chart.svg)](https://travis-ci.org/mazipan/vue-doughnut-chart) [![Dependabot Status](https://api.dependabot.com/badges/status?host=github&repo=mazipan/vue-doughnut-chart)](https://dependabot.com)

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

| Props Name            | Type      | Default Value |Description                                  | 
|-----------------------|-----------|---------------|---------------------------------------------|
| `percent`             | Number    |  0            |                                             |
| `foregroundColor`     | String    |  '#1993ff'    |                                             |
| `backgroundColor`     | String    |  '#ecf6ff'    |                                             |
| `strokeWidth`         | Number    |  10           |                                             |
| `radius`              | Number    |  85           |                                             |
| `width`               | Number    |  200          |                                             |
| `height`              | Number    |  200          |                                             |
| `classValue`          | String    |  ''           |                                             |
| `visibleValue`        | Boolean   |  false        |                                             |
| `valueCountUp`        | Boolean   |  false        |                                             |
| `valueCountUpDuration`| Number    |  2000         | Number in milliseconds                      |
| `customPercentSize`   | Number    |  40           | Percent font size in pixels (max 60)        |
| `passTextAsHtml`      | Boolean   |  false        | Allows to add simple html into label        |
| `customText`          | String    |  ''           | Label value                                 |
| `customTextColor`     | String    |  '#1993ff'    | Valid HEX color code or CSS color for label |
| `customTextSize`      | Number    |  15           | Label font size in pixels (max 22)          |

-----

Bringing to NPM Registry by Irfan Maulana © 2018
