<template>
  <div class="main">
    <img
      src="../assets/scoreBar/score-and-time-panel.png"
      alt="score-bar"
      class="score-bg"
    />

    <img
      src="../assets/scoreBar/time-circle.png"
      alt="score-bar"
      class="time-circle"
    />
    <p class="score">{{ scoreNumber }}</p>
    <p class="time">{{ timeNumber }}</p>
  </div>
</template>

<script>
export default {
  props: {
    scoreNumber: Number,
    timeNumber: Number,
  },
};
</script>

<style scoped>
@import url("https://fonts.googleapis.com/css2?family=Rammetto+One&display=swap");

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

.time-circle {
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

.time {
  font-family: "Rammetto One", cursive;
  position: absolute;
  top: 29%;
  left: 50%;
  transform: translateX(-52%);
  font-size: 48px;
  color: #fff;
}
</style>