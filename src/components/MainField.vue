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
        <img
          src="../assets/blocks/blue.png"
          alt="red"
          v-if="brick.color === 'blue'"
          class="brick"
        />
        <img
          src="../assets/blocks/green.png"
          alt="green"
          v-if="brick.color === 'green'"
          class="brick"
        />
        <img
          src="../assets/blocks/purple.png"
          alt="purple"
          v-if="brick.color === 'purple'"
          class="brick"
        />
        <img
          src="../assets/blocks/red.png"
          alt="red"
          v-if="brick.color === 'red'"
          class="brick"
        />
        <img
          src="../assets/blocks/yellow.png"
          alt="yellow"
          v-if="brick.color === 'yellow'"
          class="brick"
        />
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
      console.log("base: ", idx);
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
            console.log("Nindex: ", nIndex);
            if (nIndex === index) {
              isExists = true;
            }
          });

          if (!isExists) {
            nList.push(index);
            this.checkNList.push(index);
            console.log("recursive: ", index);
            nList.push(...this.neighboursList(index));
          }
          isExists = false;
        }
      });

      return nList;
    },
    clickBrick(idx) {
      // this.bricksArray[idx].top += 50;
      let neighboursList = this.neighboursList(idx);
      // neighboursList.push(idx);
      // this.checkNList.push(idx);
      console.log("neighboursList: ", neighboursList);
      neighboursList.forEach((findedIndex) => {
        this.bricksArray[findedIndex].left += 500;
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
        });
        leftC += 50;
        if ((i + 1) % 9 === 0 && i !== 0) {
          console.log("%%%9: ", i);
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
.main {
  background-color: #001e3b;
  display: grid;
  justify-content: center;
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
  transition: all 0.5s;
}
</style>
