<template>
  <div class='doughnut_chart' style='position:relative;'>
    <svg :width='width' :height='height' viewBox='0 0 200 200' style='stroke-linecap:round;'>
      <!-- Background circle -->
      <path :d='dBg' fill='transparent' :stroke='backgroundColor' :stroke-width='strokeWidth' />
      <!-- Move to start position, start drawing arc -->
      <path :d='d' fill='transparent' :stroke='foregroundColor' :stroke-width='strokeWidth' />
    </svg>
    <div
        v-if='visibleValue'
        class='value-container'
    >
      <template v-if='passTextAsHtml'>
        <div
            v-if='customText.length'
            :style='customTextStyle'
            v-html='customText'
        ></div>
      </template>
      <template v-else>
        <div
            v-if='customText.length'
            v-html='customText'
            :class='classValue'
            :style='customTextStyle'
        />
        <p
            v-else-if='visibleEmptyText'
            :class='classValue'
            :style='valueStyle'
        >
          {{ emptyText }}
        </p>
        <h1
            v-else
            :class='classValue'
            :style='valueStyle'
        >
          {{ percent + '%' }}
        </h1>
      </template>
    </div>
  </div>
</template>

<script>
export default {
  name: "DoughnutChart",
  props: {
    percent: {
      default: 0
    },
    foregroundColor: {
      type: String,
      default: "#1993ff"
    },
    backgroundColor: {
      type: String,
      default: "#ecf6ff"
    },
    strokeWidth: {
      type: Number,
      default: 10
    },
    radius: {
      type: Number,
      default: 85
    },
    width: {
      type: Number,
      default: 200
    },
    height: {
      type: Number,
      default: 200
    },
    visibleValue: {
      type: Boolean,
      default: false
    },
    emptyText: {
      type: String,
      default: ""
    },
    classValue: {
      type: String,
      default: ""
    },
    customText: {
      type: String,
      default: ""
    },
    passTextAsHtml: {
      type: Boolean,
      default: false
    },
    customPercentSize: {
      type: Number,
      default: 40,
      required: false
    }
  },
  data() {
    return {};
  },
  computed: {
    // If more than 50% filled we need to switch arc drawing mode from less than 180 deg to more than 180 deg
    largeArc() {
      return parseInt(this.percent) < 50 ? 0 : 1;
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
      const degrees = ( this.percent / 100 ) * 360;
      const value = degrees - 180; // Turn the circle 180 degrees counter clockwise

      return ( value * Math.PI ) / 180;
    },
    // If we reach full circle we need to complete the circle, this ties into the rounding error in X coordinate above
    z() {
      return parseInt(this.percent) === 100 ? "z" : "";
    },
    dBg() {
      return `M ${ this.x } ${ this.y } A ${ this.radius } ${ this.radius } 0 1 1 ${ this
          .x - 0.0001 } ${ this.y } z`;
    },
    d() {
      return `M ${ this.x } ${ this.y } A ${ this.radius } ${ this.radius } 0 ${ this.largeArc } 1 ${ this.endX } ${ this.endY } ${ this.z }`;
    },
    valueFromBottom() {
      if (this.strokeWidth < 15) {
        return this.height / 2 - this.strokeWidth / 1.5;
      } else {
        return this.height / 2 - this.strokeWidth / 3;
      }
    },
    valueFromLeft() {
      if (this.strokeWidth < 15) {
        return this.width / 2 - this.strokeWidth;
      } else {
        return this.width / 2 - this.strokeWidth;
      }
    },
    valueStyle() {
      let percentSize = (this.customPercentSize && this.customPercentSize < 60) ? this.customPercentSize : false
      return {
        color: this.foregroundColor,
        fontSize: `${ percentSize }px`
      };
    },
    visibleEmptyText() {
      return parseInt(this.percent) === 0 && this.emptyText !== "";
    },
    customTextStyle() {
      return {
        color: this.foregroundColor,
        width: `${ this.width - 6 * this.strokeWidth }px`,
        height: `${ this.height / 3 }px`,
        bottom: `${ this.height / 2 - this.strokeWidth * 3 }px`,
        left: `${ this.valueFromLeft / 3 }px`,
        position: "absolute",
        verticalAlign: "middle",
        wordBreak: "break-all",
        wordWrap: "break-word",
        fontSize: "14px",
        textAlign: "center"
      };
    }
  }
};
</script>
<style lang='scss' scoped>
h1 {
  margin: 0;
  padding: 0;
}

.value-container {
  display: flex;
  flex-flow: column nowrap;
  align-items: center;
  justify-content: center;
  width: 100%;
  height: 100%;
  position: absolute;
  top: 0;
  left: 0;
}
</style>
