<template>
  <div class="main">
    <p>hello</p>
    <div class="field">
      <!-- <img
        v-for="(brick, index) in bricksArray"
        :key="index"
        alt="brick"
        class="brick"
        :src="require(brick.color)"
        :style="{
          top: brick.top + 'px',
          left: brick.left + 'px',
        }"
      /> -->

      <div
        v-for="(brick, index) in bricksArray"
        :key="index"
        alt="brick"
        class="brick"
        :style="{
          top: brick.top + 'px',
          left: brick.left + 'px',
        }"
        @click="sayAlert(brick), moveBrick(index)"
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
      bricksArray: [
        {
          color: "red",
          top: 12,
          left: 12,
        },
        {
          color: "blue",
          top: 62,
          left: 62,
        },
        {
          color: "green",
          top: 12,
          left: 62,
        },
        {
          color: "green",
          top: 12,
          left: 112,
        },
        {
          color: "green",
          top: 12,
          left: 162,
        },
        {
          color: "green",
          top: 12,
          left: 212,
        },
        {
          color: "green",
          top: 12,
          left: 262,
        },
        {
          color: "green",
          top: 12,
          left: 312,
        },
        {
          color: "green",
          top: 12,
          left: 362,
        },
        {
          color: "green",
          top: 12,
          left: 412,
        },
        {
          color: "blue",
          top: 112,
          left: 62,
        },
        {
          color: "blue",
          top: 162,
          left: 62,
        },
        {
          color: "blue",
          top: 212,
          left: 62,
        },
        {
          color: "blue",
          top: 262,
          left: 62,
        },
        {
          color: "blue",
          top: 312,
          left: 62,
        },
        {
          color: "blue",
          top: 362,
          left: 62,
        },
        {
          color: "blue",
          top: 412,
          left: 62,
        },
        {
          color: "blue",
          top: 462,
          left: 62,
        },
      ],
    };
  },
  methods: {
    sayAlert(info) {
      console.log(info);
    },
    moveBrick(idx) {
      this.bricksArray[idx].top += 50;
    },
  },
  created() {
    console.log(this.bricksArray);
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
p {
  color: yellowgreen;
}

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
