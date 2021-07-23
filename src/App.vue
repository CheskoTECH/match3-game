<template>
  <div class="main">
    <MainField />
  </div>
</template>

<script>
import MainField from "./components/MainField.vue";

export default {
  name: "App",
  components: {
    MainField,
  },
};
</script>

<style>
.main {
}

* {
  padding: 0;
  margin: 0;
}
</style>
