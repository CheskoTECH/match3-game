<template>
  <div class="main">
    <ProgressBar
      class="progress-bar"
      :progressNumber="(435 / maxScore) * scoreNumber"
    />
    <div class="field">
      <score-bar
        class="score-bar"
        :scoreNumber="scoreNumber"
        :timeNumber="timeNumber"
      ></score-bar>
      <div
        v-for="(brick, index) in bricksArray"
        :key="index"
        alt="brick"
        class="brick"
        :style="{
          top: brick.top + 'px',
          left: brick.left + 'px',
        }"
        @click="clickBrick(index)"
      >
        <transition name="bounce">
          <img
            src="../assets/blocks/blue.png"
            alt="red"
            v-if="brick.color === 'blue' && brick.show"
            class="brick"
          />

          <img
            src="../assets/blocks/green.png"
            alt="green"
            v-if="brick.color === 'green' && brick.show"
            class="brick"
          />
          <img
            src="../assets/blocks/purple.png"
            alt="purple"
            v-if="brick.color === 'purple' && brick.show"
            class="brick"
          />
          <img
            src="../assets/blocks/red.png"
            alt="red"
            v-if="brick.color === 'red' && brick.show"
            class="brick"
          />
          <img
            src="../assets/blocks/yellow.png"
            alt="yellow"
            v-if="brick.color === 'yellow' && brick.show"
            class="brick"
          />
        </transition>
      </div>
    </div>
    <!-- <img src="../assets/field.png" alt="field" class="field" /> -->
  </div>
</template>

<script>
import ProgressBar from "./ProgressBar.vue";
import ScoreBar from "./ScoreBar.vue";

export default {
  name: "MainField",
  components: {
    ProgressBar,
    ScoreBar,
  },
  data() {
    return {
      bricksArray: [],
      checkNList: [],
      scoreNumber: 0,
      maxScore: 1000,
      timeNumber: 90,
    };
  },
  methods: {
    neighboursList(idx) {
      let nList = [];
      let topOfClickedBrick = this.bricksArray[idx].top;
      let leftOfClickedBrick = this.bricksArray[idx].left;
      let clickedBrickColor = this.bricksArray[idx].color;

      this.bricksArray.forEach((brick, index) => {
        if (
          ((topOfClickedBrick - 50 === brick.top &&
            leftOfClickedBrick === brick.left) ||
            (topOfClickedBrick === brick.top &&
              leftOfClickedBrick - 50 === brick.left) ||
            (topOfClickedBrick + 50 === brick.top &&
              leftOfClickedBrick === brick.left) ||
            (topOfClickedBrick === brick.top &&
              leftOfClickedBrick + 50 === brick.left)) &&
          brick.color === clickedBrickColor
        ) {
          let isExists = false;
          this.checkNList.forEach((nIndex) => {
            if (nIndex === index) {
              isExists = true;
            }
          });

          if (!isExists) {
            nList.push(index);
            this.checkNList.push(index);
            nList.push(...this.neighboursList(index));
          }
          isExists = false;
        }
      });
      return nList;
    },
    showAndDropClikedBricks() {
      let colorsArray = ["blue", "green", "purple", "red", "yellow"];

      let indexes = [];
      this.bricksArray.forEach((brick, index) => {
        if (brick.show === false) {
          indexes.push(index);
        }
      });

      indexes.forEach((idx) => {
        let topestPoint = 1000;
        this.bricksArray.forEach((brick) => {
          if (this.bricksArray[idx].left === brick.left && brick.show) {
            if (brick.top - 50 < topestPoint) {
              topestPoint = brick.top - 50;
            }
          }
        });
        this.bricksArray[idx].top = topestPoint;
        this.bricksArray[idx].show = true;
        this.bricksArray[idx].color = colorsArray[this.getRandomInt(0, 5)];
      });
    },
    dropLeftBricksDown() {
      let isMoved = false;

      for (let i = this.bricksArray.length - 1; i >= 0; i--) {
        let move = true;
        let iBrick = this.bricksArray[i];
        if (iBrick.show) {
          for (let j = this.bricksArray.length - 1; j >= 0; j--) {
            let jBrick = this.bricksArray[j];
            if (jBrick.show) {
              if (
                iBrick.top + 50 === jBrick.top &&
                iBrick.left === jBrick.left
              ) {
                move = false;
              }
            }
          }
        }
        if (move === true && iBrick.top !== 462 && iBrick.show === true) {
          this.bricksArray[i].top += 50;
          isMoved = true;
        }
      }

      if (isMoved) {
        this.dropLeftBricksDown();
      }

      setTimeout(() => {
        this.showAndDropClikedBricks();
      }, 300); // 900
    },
    clickBrick(idx) {
      let neighboursList = this.neighboursList(idx);
      this.checkNList = [];
      neighboursList.forEach((findedIndex) => {
        this.bricksArray[findedIndex].show = false;
        setTimeout(() => {
          this.bricksArray[findedIndex].top -= 500;
          this.dropLeftBricksDown();
        }, 700);
      });

      this.addScore(neighboursList.length);
    },
    addScore(amountOfClickedBricks) {
      let newScore = amountOfClickedBricks * this.getRandomInt(1, 10);

      if (this.scoreNumber + newScore < this.maxScore) {
        this.scoreNumber += newScore;
      } else {
        this.scoreNumber = 1000;
      }
    },
    getRandomInt(min, max) {
      min = Math.ceil(min);
      max = Math.floor(max);
      return Math.floor(Math.random() * (max - min)) + min; //Максимум не включается, минимум включается
    },
    createNewBricks() {
      let colorsArray = ["blue", "green", "purple", "red", "yellow"];

      let topC = 12;
      let leftC = 12;

      for (let i = 0; i < 90; i++) {
        this.bricksArray.push({
          color: colorsArray[this.getRandomInt(0, 5)],
          top: topC,
          left: leftC,
          show: true,
        });
        leftC += 50;
        if ((i + 1) % 9 === 0 && i !== 0) {
          topC += 50;
          leftC = 12;
        }
      }
    },
    timeTracking() {
      let timer = setInterval(() => {
        this.timeNumber -= 1;
      }, 1000);
      setTimeout(() => {
        clearInterval(timer);
      }, this.timeNumber * 1000);
    },
  },
  created() {
    this.createNewBricks();
    this.timeTracking();
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.bounce-enter-active {
  animation: bounce-in 0.5s;
}

.bounce-leave-active {
  animation: bounce-in 0.5s reverse;
}
@keyframes bounce-in {
  0% {
    transform: scale(0);
  }
  50% {
    transform: scale(1.2);
  }
  100% {
    transform: scale(1);
  }
}

.main {
  background-color: #001e3b;
  display: grid;
  justify-content: center;
  align-content: center;
  height: 100vh;
}

.field {
  height: 525px;
  /* border: 1px solid red; */
  width: 475px;
  background-image: url("../assets/field.png");
  background-size: cover;
  background-position: top;
  position: relative;
}

.brick {
  position: absolute;
  width: 50px;
  height: 50px;
  transition: all 0.4s;
}

.progress-bar {
  position: absolute;
  top: 0;
  left: 50%;
  transform: translateX(-50%);
  width: 475px;
  height: 100px;
}

.score-bar {
  position: absolute;
  top: 0;
  left: 550px;
  height: 360px;
  width: 320px;
}
</style>
