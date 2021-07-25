<template>
  <div class="main">
    <img
      src="../assets/scoreBar/score-panel.png"
      alt="score-bar"
      class="score-bg"
    />
    <p class="time-left" v-if="!isTimeStopped">{{ timeNumber }}</p>
    <p class="time-left" v-if="isTimeStopped">{{ timeFreezed }}</p>
    <img
      src="../assets/scoreBar/time-circle.png"
      alt="score-bar"
      class="steps-circle"
    />
    <p class="score">{{ scoreNumber }}</p>
    <p class="steps">{{ stepsLeft }}</p>
  </div>
</template>

<script>
export default {
  props: {
    scoreNumber: Number,
    timeNumber: Number,
    stepsLeft: Number,
    isTimeStopped: Boolean,
  },
  data() {
    return {
      timeFreezed: 0,
    };
  },
  watch: {
    isTimeStopped: function (value) {
      if (value === true) {
        this.timeFreezed = this.timeNumber;
      }
    },
  },
};
</script>

<style scoped>
.main {
  box-sizing: border-box;
  position: relative;
}

.score-bg {
  position: absolute;
  top: 0;
  width: 100%;
  height: 100%;
}

.time-left {
  position: absolute;
  font-size: 20px;
  color: #fff;
  font-family: "Rammetto One", cursive;
  left: 200px;
  top: 8px;
}

.steps-circle {
  position: absolute;
  top: 52px;
  width: 175px;
  left: 50%;
  transform: translateX(-50%);
}

.score {
  font-family: "Rammetto One", cursive;
  position: absolute;
  top: 74%;
  left: 50%;
  transform: translateX(-52%);
  font-size: 32px;
  color: #fff;
}

.steps {
  font-family: "Rammetto One", cursive;
  position: absolute;
  top: 29%;
  left: 50%;
  transform: translateX(-52%);
  font-size: 48px;
  color: #fff;
}
</style>