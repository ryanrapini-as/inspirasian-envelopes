<template>
  <div class="envelope">
    <div class="wrapper" :class="{ shakeable: !stopAlexClickingSoDamnFast }">
      <!-- <span style="font-size: 10px"
      >{{ beginFlip }}<br />{{ halfFlipped }}<br />{{ flipped }} <br /> {{ opened }}</span
    > -->
      <div class="surprises" :class="{ shown: particles }">
        <div class="fly">福</div>
        <div class="fly">888</div>
        <div class="fly">祿</div>
        <div class="fly">666</div>
        <div class="fly">999</div>
        <div class="fly">
          <i class="hearts"></i>
        </div>
        <div class="fly">合</div>
        <div class="fly">
          <i class="hearts"></i>
        </div>
      </div>
      <div
        class="bonus jumping"
        :class="{ rotate: beginFlip, rotateBack: halfFlipped }"
        @mouseenter="showParticles()"
        @mouseleave="hideParticles()"
        @click="clickEnvelope"
      ></div>
      <div class="arrow-up" :class="{ opened: opened }" v-if="flipped"></div>
      <div
        class="paper"
        :class="{ flipped: flipped, opened: opened }"
        v-if="flipped"
      >
        <div class="message">
          <template v-if="paper.startsWith('*')">
            <img :src="paper.slice(1) + '.svg'" alt="bonus" />
          </template>
          <template v-else> {{ paper }}</template>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "Envelope",
  emits: ["onClicked"],
  props: {
    paper: {
      // Specify the expected data type for the prop
      type: String,
      // Make the prop required (optional)
      required: true,
    },
    isFlipped: {
      type: Boolean,
      default: false,
    },
    stopAlexClickingSoDamnFast: {
      type: Boolean,
      default: false,
    },
  },
  data() {
    return {
      particles: false,
      timer: null,
      clicked: false,

      beginFlip: false,
      halfFlipped: false,
      flipped: false,
      opened: false,
    };
  },
  watch: {
    isFlipped() {
      this.flipEnvelope();
    },
  },
  methods: {
    clickEnvelope() {
      if (!this.isFlipped && !this.stopAlexClickingSoDamnFast) {
        this.$emit("onClicked");
      }
    },
    flipEnvelope() {
      if (this.flipped == true) {
        this.beginFlip = false;
        this.halfFlipped = false;
        this.flipped = false;
        this.opened = false;
        return;
      }
      const time = 400;
      this.beginFlip = true;
      setTimeout(() => {
        this.halfFlipped = true;
        setTimeout(() => {
          this.flipped = true;
          setTimeout(() => {
            this.opened = true;
          }, time + 100);
        }, time);
      }, time);
    },
    showParticles() {
      this.particles = true;
      clearTimeout(this.timer);
    },
    hideParticles() {
      if (this.timer) {
        clearTimeout(this.timer);
        this.timer = null;
      }
      this.timer = setTimeout(() => {
        this.particles = false;
        this.timer = null; // Reset the timer after it executes
      }, 1000);
    },
  },
};
</script>

<style>
@font-face {
  font-family: "Shufen Free Trial";
  src: "https://ryanrapini-as.github.io/inspirasian-envelopes/Shufen%20Free%20Trial.ttf";
}

a,
a:link,
a:hover,
a:active {
  text-decoration: none;
}

.envelope {
  position: relative;
  display: inline-block;
  height: 200px;
  width: 150px;
}

.rotate {
  transform: rotateY(90deg);
  animation: rotateAnimation 0.4s linear;
  -webkit-animation: rotateAnimation 0.4s linear;
}
.rotateBack {
  transform: rotateY(180deg);
  animation: rotateAnimation 0.4s linear;
  -webkit-animation: rotateAnimation 0.4s linear;
}

.wrapper {
  position: absolute;
  top: 0;
  right: 0;
  bottom: 0;
  left: 0;
  margin: auto;
  width: 150px;
  height: 150px;
  text-align: center;
}

.wrapper h1 {
  margin-top: 15px;
  font-size: 12px;
  color: #f00;
}

.paper .message {
  font-family: "Shufen Free Trial", sans-serif;
  font-size: 18px;
  padding: 4px;
  padding-top: 10px;
}

.paper .message img {
  width: 75%;
  margin-left: auto;
  margin-right: auto;
}

.wrapper .paper {
  position: absolute;
  bottom: 5px;
  border: 1px solid #e8e8e8;
  left: 50%;
  margin-left: -38px;
  width: 74px;
  height: 90px;
  text-align: center;
  overflow: hidden;
  border-radius: 0px;
  background-color: #fff;
  transition: all 0.4s ease;
  cursor: pointer;
  z-index: 2;
}

.paper.opened {
  bottom: 75px !important;
}

.wrapper .bonus {
  position: absolute;
  bottom: 5px;
  left: 50%;
  margin-left: -40px;
  width: 80px;
  height: 100px;
  text-align: center;
  overflow: hidden;
  border-radius: 0px;
  background-color: #f00;
  transition: all 0.4s ease;
  cursor: pointer;
  z-index: 3;
}

.wrapper .bonus:after {
  position: absolute;
  top: 15px;
  left: 0;
  content: "";
  width: 80px;
  height: 20px;
  border-radius: 50%;
  box-shadow: 0 3px 0 2px #ff0;
  z-index: 3;
}

.wrapper .bonus:before {
  position: absolute;
  top: 22px;
  left: 50%;
  margin-left: -15px;
  content: "BEP";
  width: 30px;
  height: 30px;
  line-height: 35px;
  font-size: 12px;
  font-weight: 700;
  color: #f00;
  background-color: #ff0;
  border-radius: 50%;
  z-index: 4;
}

.wrapper .bonus.rotateBack.rotate:before,
.wrapper .bonus.rotateBack.rotate:after {
  display: none;
}

.wrapper .bonus {
  background-color: #d00;
  /* animation-name: shaking; */
}

.wrapper.shakeable .bonus:hover:not(.rotate):not(.rotateBack) {
  animation-name: shaking;
}

.surprises {
  opacity: 0%;
  transition: opacity 0.5s ease-in-out;
}

.surprises.shown {
  opacity: 100%;
}

.arrow-up {
  position: absolute;
  top: 45px;
  left: 50%;
  margin-left: -40px;
  width: 0;
  height: 0;
  border-top: 40px solid red;
  border-left: 40px solid transparent;
  border-right: 40px solid transparent;
  transform-origin: 50% 0%;
  transform: rotateX(180deg);
  animation: openEnvelopeAnimation 0.25 linear;
  -webkit-animation: openEnvelopeAnimation 0.25s linear;
  z-index: 3;
}
.arrow-up.opened {
  z-index: 2 !important;
}

@keyframes openEnvelopeAnimation {
  from {
    transform: rotateX(0deg);
  }
  to {
    transform: rotateX(180deg);
  }
}

.surprises .fly {
  position: absolute;
  top: 45px;
  left: 0;
  margin-left: -25px;
  right: 0;
  color: #f37476;
  z-index: 0;
  opacity: 0;
  font-size: 10px;
  animation-duration: 1.65s;
  animation-timing-function: ease-in-out;
  animation-iteration-count: infinite;
  animation-direction: normal;
  animation-delay: 0;
  animation-play-state: running;
  animation-name: flying;
}

.surprises .fly:nth-child(1) {
  animation-delay: 1.5s;
  left: -20px;
}

.surprises .fly:nth-child(2) {
  animation-delay: 1.2s;
  left: 30;
}

.surprises .fly:nth-child(3) {
  animation-delay: 0.9s;
  left: 15px;
}

.surprises .fly:nth-child(4) {
  animation-delay: 1.6s;
  left: 60px;
}

.surprises .fly:nth-child(5) {
  animation-delay: 0.3s;
  left: -45px;
}

.surprises .fly:nth-child(6) {
  animation-delay: 0.15s;
  left: 45px;
}

.surprises .fly:nth-child(7) {
  animation-delay: 1.15s;
  left: 85px;
}

.surprises .fly:nth-child(8) {
  animation-delay: 0.75s;
  left: 0px;
}

.hearts {
  position: relative;
}

.hearts:before,
.hearts:after {
  position: absolute;
  content: "";
  width: 6px;
  height: 10px;
  background: #ff370f;
  border-radius: 3px 3px 0 0;
  transform: rotate(-45deg);
  transform-origin: 10% 50%;
}

.hearts:after {
  left: 0;
  transform: rotate(45deg);
  transform-origin: 110% 68%;
}

.jumping:not(.rotate) {
  animation-duration: 0.6s;
  animation-time-function: ease;
  animation-delay: 0;
  animation-iteration-count: infinite;
  animation-direction: normal;
  animation-play-state: running;
}

@-moz-keyframes jumping {
  0% {
    bottom: 5px;
    transform: rotate(-10deg) scale(0.98);
  }

  10% {
    bottom: 30px;
    transform: rotate(0deg) scale(1);
  }

  50% {
    bottom: 5px;
    transform: rotate(10deg) scale(0.98);
  }

  60% {
    bottom: 30px;
    transform: rotate(0deg) scale(1);
  }

  100% {
    bottom: 5px;
    transform: rotate(-10deg) scale(0.98);
  }
}

@-webkit-keyframes jumping {
  0% {
    bottom: 5px;
    transform: rotate(-10deg) scale(0.98);
  }

  10% {
    bottom: 30px;
    transform: rotate(0deg) scale(1);
  }

  50% {
    bottom: 5px;
    transform: rotate(10deg) scale(0.98);
  }

  60% {
    bottom: 30px;
    transform: rotate(0deg) scale(1);
  }

  100% {
    bottom: 5px;
    transform: rotate(-10deg) scale(0.98);
  }
}

@-o-keyframes jumping {
  0% {
    bottom: 5px;
    transform: rotate(-10deg) scale(0.98);
  }

  10% {
    bottom: 30px;
    transform: rotate(0deg) scale(1);
  }

  50% {
    bottom: 5px;
    transform: rotate(10deg) scale(0.98);
  }

  60% {
    bottom: 30px;
    transform: rotate(0deg) scale(1);
  }

  100% {
    bottom: 5px;
    transform: rotate(-10deg) scale(0.98);
  }
}

@keyframes jumping {
  0% {
    bottom: 5px;
    transform: rotate(-10deg) scale(0.98);
  }

  10% {
    bottom: 30px;
    transform: rotate(0deg) scale(1);
  }

  50% {
    bottom: 5px;
    transform: rotate(10deg) scale(0.98);
  }

  60% {
    bottom: 30px;
    transform: rotate(0deg) scale(1);
  }

  100% {
    bottom: 5px;
    transform: rotate(-10deg) scale(0.98);
  }
}

@-moz-keyframes shaking {
  0% {
    margin-left: -35px;
  }

  10% {
    margin-left: -45px;
  }

  20% {
    margin-left: -35px;
  }

  30% {
    margin-left: -45px;
  }

  40% {
    margin-left: -35px;
  }

  50% {
    margin-left: -40px;
  }

  65% {
    bottom: 20px;
  }

  100% {
    bottom: 5px;
  }
}

@-webkit-keyframes shaking {
  0% {
    margin-left: -35px;
  }

  10% {
    margin-left: -45px;
  }

  20% {
    margin-left: -35px;
  }

  30% {
    margin-left: -45px;
  }

  40% {
    margin-left: -35px;
  }

  50% {
    margin-left: -40px;
  }

  65% {
    bottom: 20px;
  }

  100% {
    bottom: 5px;
  }
}

@-o-keyframes shaking {
  0% {
    margin-left: -35px;
  }

  10% {
    margin-left: -45px;
  }

  20% {
    margin-left: -35px;
  }

  30% {
    margin-left: -45px;
  }

  40% {
    margin-left: -35px;
  }

  50% {
    margin-left: -40px;
  }

  65% {
    bottom: 20px;
  }

  100% {
    bottom: 5px;
  }
}

@keyframes shaking {
  0% {
    margin-left: -35px;
  }

  10% {
    margin-left: -45px;
  }

  20% {
    margin-left: -35px;
  }

  30% {
    margin-left: -45px;
  }

  40% {
    margin-left: -35px;
  }

  50% {
    margin-left: -40px;
  }

  65% {
    bottom: 20px;
  }

  100% {
    bottom: 5px;
  }
}

@-moz-keyframes flying {
  0% {
    transform: rotate(20deg);
    opacity: 0;
  }

  50% {
    transform: rotate(-20deg);
    opacity: 1;
  }

  100% {
    top: -60px;
    transform: rotate(20deg);
    opacity: 0;
  }
}

@-webkit-keyframes flying {
  0% {
    transform: rotate(20deg);
    opacity: 0;
  }

  50% {
    transform: rotate(-20deg);
    opacity: 1;
  }

  100% {
    top: -60px;
    transform: rotate(20deg);
    opacity: 0;
  }
}

@-o-keyframes flying {
  0% {
    transform: rotate(20deg);
    opacity: 0;
  }

  50% {
    transform: rotate(-20deg);
    opacity: 1;
  }

  100% {
    top: -60px;
    transform: rotate(20deg);
    opacity: 0;
  }
}

@keyframes flying {
  0% {
    transform: rotate(20deg);
    opacity: 0;
  }

  50% {
    transform: rotate(-20deg);
    opacity: 1;
  }

  100% {
    top: -60px;
    transform: rotate(20deg);
    opacity: 0;
  }
}

@keyframes rotateAnimation {
  from {
    transform: rotateY(45deg);
  }
  to {
    transform: rotateY(225deg);
  }
}

@keyframes fadeIn {
  from {
    opacity: 1;
  }
  to {
    opacity: 0;
  }
}
</style>
