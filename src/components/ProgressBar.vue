<template>
  <div class="main">
    <img
      src="../assets/progressBar/RoundedRectangle.png"
      alt="progress-bar"
      class="progress-bg"
    />

    <img
      src="../assets/progressBar/barfield.png"
      alt="progress-bar"
      class="barfield"
    />

    <img
      src="../assets/progressBar/progress-text.png"
      alt="progress-bar"
      class="progress-text"
    />

    <img
      src="../assets/progressBar/progressbar.png"
      alt="progress-bar"
      class="progress-bar"
      :style="{
        width: progressNumber + 'px',
      }"
    />

    <img
      src="../assets/bonuses/money-field.png"
      alt="money"
      class="money-field"
    />
    <p class="money-text">{{ money }}</p>
  </div>
</template>

<script>
export default {
  props: {
    progressNumber: Number,
    money: Number,
  },
};
</script>

<style scoped>
.main {
  box-sizing: border-box;
  position: relative;
}

.progress-bg {
  position: absolute;
  top: 0;
  width: 100%;
  height: 100%;
}

.barfield {
  position: absolute;
  top: 45px;
  width: 435px;
  height: 32px;
  left: 50%;
  transform: translateX(-50%);
}

.progress-text {
  position: absolute;
  height: 20px;
  top: 10px;
  left: 50%;
  transform: translateX(-50%);
}

.progress-bar {
  position: absolute;
  top: 45px;
  height: 30px;
  border-radius: 40px;
  left: 19px;
  transition: all 0.5s;
}

.money-field {
  position: absolute;
  top: 20px;
  left: 110%;
  height: 40px;
}

.money-text {
  position: absolute;
  top: 22px;
  left: 120%;
  font-size: 22px;
  color: #fff;
  font-family: "Rammetto One", cursive;
}
</style>