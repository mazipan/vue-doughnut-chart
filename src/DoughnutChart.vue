<template>
  <div class="doughnut_chart" style="position:relative;">
    <svg
      :width="width"
      :height="height"
      viewBox="0 0 200 200">
      <!-- Background circle -->
      <path :d="dBg"
            fill="transparent"
            :stroke="backgroundColor"
            :stroke-width="strokeWidth"/>
      <!-- Move to start position, start drawing arc -->
      <path :d="d"
            fill="transparent"
            :stroke="foregroundColor"
            :stroke-width="strokeWidth"/>
    </svg>
    <div v-if="visibleValue" :style="valueStyle">
      <span v-if="visibleEmptyText" :class="classValue">{{ emptyText }}</span>
      <span v-else :class="classValue">{{ percent }}%</span>
    </div>
  </div>
</template>

<script>
export default {
  name: 'DoughnutChart',
  props: {
    percent: {
      type: Number,
      default: 0,
    },
    foregroundColor: {
      type: String,
      default: '#1993ff',
    },
    backgroundColor: {
      type: String,
      default: '#ecf6ff',
    },
    strokeWidth: {
      type: Number,
      default: 10,
    },
    radius: {
      type: Number,
      default: 85,
    },
    width: {
      type: Number,
      default: 200,
    },
    height: {
      type: Number,
      default: 200,
    },
    visibleValue: {
      type: Boolean,
      default: false,
    },
    emptyText: {
      type: String,
      default: '',
    },
    classValue: {
      type: String,
      default: '',
    },
  },
  data() {
    return {};
  },
  computed: {
    // If more than 50% filled we need to switch arc drawing mode from less than 180 deg to more than 180 deg
    largeArc() {
      return this.percent < 50 ? 0 : 1;
    },
    // Where to put x coordinate of center of circle
    x() {
      return 100;
    },
    // Where to put y coordinate of center of circle
    y() {
      return 100 - this.radius;
    },
    // Calculate X coordinate of end of arc (+ 100 to move it to middle of image)
    // add some rounding error to make arc not disappear at 100%
    endX() {
      return -Math.sin(this.radians) * this.radius + 100 - 0.0001; // eslint-disable-line no-mixed-operators
    },
    // Calculate Y coordinate of end of arc (+ 100 to move it to middle of image)
    endY() {
      return Math.cos(this.radians) * this.radius + 100; // eslint-disable-line no-mixed-operators
    },
    // Calculate length of arc in radians
    radians() {
      const degrees = (this.percent / 100) * 360;
      const value = degrees - 180; // Turn the circle 180 degrees counter clockwise

      return (value * Math.PI) / 180;
    },
    // If we reach full circle we need to complete the circle, this ties into the rounding error in X coordinate above
    z() {
      return this.percent === 100 ? 'z' : '';
    },
    dBg() {
      return `M ${this.x} ${this.y} A ${this.radius} ${this.radius} 0 1 1 ${this
        .x - 0.0001} ${this.y} z`;
    },
    d() {
      return `M ${this.x} ${this.y} A ${this.radius} ${this.radius} 0 ${
        this.largeArc
      } 1 ${this.endX} ${this.endY} ${this.z}`;
    },
    valueFromBottom() {
      return (this.height / 2) - this.strokeWidth;
    },
    valueFromLeft() {
      return (this.width / 2) - this.strokeWidth;
    },
    valueStyle() {
      return {
        color: this.foregroundColor,
        bottom: `${this.valueFromBottom}px`,
        left: `${this.valueFromLeft}px`,
        position: 'absolute',
        'font-size': '18px'
      };
    },
    visibleEmptyText() {
      return ((this.percent === '0' || this.percent === 0) && this.emptyText !== '')
    }
  },
};
</script>
