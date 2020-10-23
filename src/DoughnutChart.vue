<template>
  <div class='doughnut_chart' style='position:relative;'>
    <svg :width='width' :height='height' viewBox='0 0 200 200' style='stroke-linecap:round;'>
      <!-- Background circle -->
      <path :d='dBg' fill='transparent'
            :stroke="(backgroundColor && validateColor(backgroundColor)) ? backgroundColor : '#ecf6ff'"
            :stroke-width='strokeWidth'
      />
      <!-- Move to start position, start drawing arc -->
      <path :d='d' fill='transparent'
            :stroke="(foregroundColor && validateColor(foregroundColor)) ? foregroundColor : '#1993ff'"
            :stroke-width='strokeWidth'
      />
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
        />
      </template>
      <template v-else>
        <h1
            v-if='percent'
            :class='classValue'
            :style='valueStyle'
        >
          {{ valueCountUp ? countingUpValue + '%' : percent + '%' }}
        </h1>
        <div
            v-if='customText.length'
            v-html='customText'
            :class='classValue'
            :style='customTextStyle'
        />
      </template>
    </div>
    <div
        v-else-if='customText.length'
        class='value-container'
    >
      <div
          v-if='customText.length'
          v-html='customText'
          :class='classValue'
          :style='customTextStyle'
      />
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
    classValue: {
      type: String,
      default: ""
    },
    visibleValue: {
      type: Boolean,
      default: false
    },
    valueCountUp: {
      type: Boolean,
      default: false,
      required: false
    },
    valueCountUpDuration: {
      type: Number,
      default: 2000,
      required: false
    },
    valueCountUpDelay: {
      type: Number,
      default: 500,
      required: false
    },
    customPercentSize: {
      type: Number,
      default: 40,
      required: false
    },
    passTextAsHtml: {
      type: Boolean,
      default: false
    },
    customText: {
      type: String,
      default: ""
    },
    customTextColor: {
      type: String,
      default: '',
      required: false
    },
    customTextSize: {
      type: Number,
      default: 15,
      required: false
    }
  },
  data() {
    return {
      countingUpValue: 0,
      delayTimer: null
    };
  },
  computed: {
    // If more than 50% filled we need to switch arc drawing mode from less than 180 deg to more than 180 deg
    largeArc() {
      const number = this.valueCountUp ? this.countingUpValue : this.percent
      return parseInt(number) < 50 ? 0 : 1;
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
      const number = this.valueCountUp ? this.countingUpValue : this.percent
      const degrees = ( number / 100 ) * 360;
      const value = degrees - 180; // Turn the circle 180 degrees counter clockwise

      return ( value * Math.PI ) / 180;
    },
    // If we reach full circle we need to complete the circle, this ties into the rounding error in X coordinate above
    z() {
      const number = this.valueCountUp ? this.countingUpValue : this.percent
      return parseInt(number) === 100 ? "z" : "";
    },
    dBg() {
      return `M ${ this.x } ${ this.y } A ${ this.radius } ${ this.radius } 0 1 1 ${ this
          .x - 0.0001 } ${ this.y } z`;
    },
    d() {
      return `M ${ this.x } ${ this.y } A ${ this.radius } ${ this.radius } 0 ${ this.largeArc } 1 ${ this.endX } ${ this.endY } ${ this.z }`;
    },
    valueStyle() {
      let percentColor = ( this.foregroundColor && this.validateColor(this.foregroundColor) ) ? this.foregroundColor : '#1993ff'
      let percentSize = ( this.customPercentSize && this.customPercentSize < 60 ) ? `${ this.customPercentSize }px` : false
      return {
        color: percentColor,
        fontSize: percentSize,
        margin: '0 auto'
      };
    },
    customTextStyle() {
      let textColor = ( this.customTextColor && this.validateColor(this.customTextColor) ) ? this.customTextColor : this.foregroundColor
      let textWidth = this.strokeWidth ? `calc(100% - ${ this.strokeWidth * 2 }px)` : 'calc(100% - 20px)'
      let textPadding = ( this.strokeWidth && this.strokeWidth > 7 && this.strokeWidth <= 18 ) ? `0 ${ this.strokeWidth }px` : '0 10px'
      let textSize = ( this.customTextSize && this.customTextSize <= 22 ) ? `${ this.customTextSize }px` : false
      let customTopMargin = ( this.strokeWidth && this.strokeWidth > 7 && this.strokeWidth <= 18 ) ? this.strokeWidth : 5
      return {
        color: textColor,
        width: textWidth,
        padding: textPadding,
        margin: `-${ customTopMargin }px auto 0`,
        textAlign: 'center',
        fontSize: textSize,
        wordBreak: "break-all",
        wordWrap: "break-word"
      };
    }
  },
  mounted() {
    if (this.valueCountUp && this.percent) {
      this.countUpPercent()
    }
  },
  watch: {
    percent() {
      const delay = this.valueCountUpDelay ? this.valueCountUpDelay : 500
      if (this.delayTimer) {
        clearTimeout(this.delayTimer)
        this.delayTimer = null
      }
      this.delayTimer = setTimeout(() => {
        this.countUpPercent()
      }, delay)
    }
  },
  methods: {
    validateColor(string) {
      let s = new Option().style
      s.color = string
      // must match a valid css color or hex value
      return s.color !== '' || /^#([0-9A-F]{3}){1,2}$/i.test(s.color);
    },
    countUpPercent() {
      if (this.percent === this.countingUpValue) {
        return;
      }
      const animationDuration = this.valueCountUpDuration;
      const frameDuration = 1000 / 60; // Calculate how long each 'frame' should last if we want to update the animation 60 times per second
      const totalFrames = Math.round(animationDuration / frameDuration); // Use that to calculate how many frames we need to complete the animation
      const easeOutQuad = t => t * ( 2 - t ); // An ease-out function that slows the count as it progresses
      let frame = 0; // The animation function, which takes an Element
      const counter = setInterval(() => {
        frame++
        const progress = easeOutQuad(frame / totalFrames) // Calculate our progress as a value between 0 and 1
        if (this.countingUpValue !== this.percent) {
          this.countingUpValue = Math.round(this.percent * progress) // Use the progress value to calculate the current count
        }
        if (frame === totalFrames) {
          clearInterval(counter)
        }
      }, frameDuration);

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
