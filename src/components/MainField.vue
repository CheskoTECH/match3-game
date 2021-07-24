<template>
  <div class="main">
    <div class="field">
      <div
        v-for="(brick, index) in bricksArray"
        :key="index"
        alt="brick"
        class="brick"
        :style="{
          top: brick.top + 'px',
          left: brick.left + 'px',
        }"
        @click="sayAlert(brick), clickBrick(index)"
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
export default {
  name: "MainField",
  data() {
    return {
      bricksArray: [],
      checkNList: [],
    };
  },
  methods: {
    sayAlert(info) {
      console.log(info);
    },
    neighboursList(idx) {
      let nList = [];
      // console.log("base: ", idx);
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
            // console.log("Nindex: ", nIndex);
            if (nIndex === index) {
              isExists = true;
            }
          });

          if (!isExists) {
            nList.push(index);
            this.checkNList.push(index);
            // console.log("recursive: ", index);
            nList.push(...this.neighboursList(index));
          }
          isExists = false;
        }
      });
      // this.checkNList = [];
      return nList;
    },
    showAndDropClikedBricks() {
      let indexes = [];
      this.bricksArray.forEach((brick, index) => {
        if (brick.show === false) {
          indexes.push(index);
        }
        // brick.show = true
      });
      // this.dropLeftBricksDown();

      console.log("indexes: ", indexes);

      let once = false;

      // indexes.forEach((idx) => {
      //   this.bricksArray.forEach((brick) => {
      //     if (this.bricksArray[idx].left === brick.left) {
      //       if (!once) {
      //         console.log("brtop: ", brick.top, idx);
      //         this.bricksArray[idx].top = brick.top - 50;
      //         this.bricksArray[idx].show = true;
      //         once = true;
      //       }
      //     }
      //   });
      //   // once = false;
      // });

      indexes.forEach((idx) => {
        let topestPoint = 1000;
        this.bricksArray.forEach((brick) => {
          if (this.bricksArray[idx].left === brick.left && brick.show) {
            if (brick.top - 50 < topestPoint) {
              topestPoint = brick.top - 50;
            }
            if (!once) {
              // console.log("brtop: ", brick.top, idx);
              // console.log("-50: ", brick.top - 50);
              // this.bricksArray[idx].top = brick.top - 50;
              // this.bricksArray[idx].show = true;
              once = true;
            }
          }
        });
        console.log("topest: ", topestPoint);
        this.bricksArray[idx].top = topestPoint;
        this.bricksArray[idx].show = true;

        // once = false;
      });

      // if (once) {
      //   setTimeout(() => {
      //     this.showAndDropClikedBricks();
      //   }, 800);
      // }
    },
    dropLeftBricksDown() {
      // let indexes = [];
      // console.log("CALL");
      let isMoved = false;

      for (let i = 0; i < this.bricksArray.length; i++) {
        let move = true;
        let iBrick = this.bricksArray[i];
        if (iBrick.show) {
          // console.log("test: ", iBrick);

          for (let j = i + 1; j < this.bricksArray.length; j++) {
            let jBrick = this.bricksArray[j];
            if (jBrick.show) {
              if (
                iBrick.top + 50 === jBrick.top &&
                iBrick.left === jBrick.left
              ) {
                console.log(jBrick.top, " ", iBrick.top);
                // indexes.push(index);
                move = false;
              }
            }
          }
        }
        if (move === true && iBrick.top !== 462 && iBrick.show === true) {
          // console.log("2:", this.bricksArray[i].top, "MOVE: ", move);

          // let block = false;
          // this.bricksArray.forEach((brick) => {
          //   if (
          //     this.bricksArray[i].top + 50 === brick.top &&
          //     brick.left === brick.left
          //   ) {
          //     block = true;
          //   }
          // });

          // if (!block) {
          //   }

          this.bricksArray[i].top += 50;
          // console.log(
          //   "22:",
          //   this.bricksArray[i].top,
          //   " ",
          //   this.bricksArray[i].color
          // );
          isMoved = true;
        }
      }

      if (isMoved) {
        this.dropLeftBricksDown();

        setTimeout(() => {
          this.showAndDropClikedBricks();
        }, 900);
        // setTimeout(() => {
        //   this.bricksArray.forEach((brick) => {
        //     if (brick.show === false) {
        //       console.log("TOP1: ", brick.top);
        //       // brick.top += 50;
        //       // brick.top = 12;
        //       brick.show = true;
        //       console.log("TOP2: ", brick.top);
        //     }
        //   });
        //   this.dropLeftBricksDown();
        //   // this.dropLeftBricksDown();
        // }, 1000);
      }

      // console.log(indexes);
    },
    clickBrick(idx) {
      // this.bricksArray[idx].top += 50;
      let neighboursList = this.neighboursList(idx);
      this.checkNList = [];
      // neighboursList.push(idx);
      // this.checkNList.push(idx);
      // console.log("neighboursList: ", neighboursList);
      neighboursList.forEach((findedIndex) => {
        this.bricksArray[findedIndex].show = false;
        setTimeout(() => {
          // this.bricksArray.splice(findedIndex, 1);
          this.bricksArray[findedIndex].top -= 500;
          this.dropLeftBricksDown();
          // this.bricksArray[findedIndex].show = true;
        }, 700);
      });
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
          // console.log("%%%9: ", i);
          topC += 50;
          leftC = 12;
        }
      }
    },
  },
  created() {
    this.createNewBricks();
    console.log(this.bricksArray);
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
  border: 1px solid red;
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
</style>
