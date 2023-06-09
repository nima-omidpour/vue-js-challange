<template>
  <div id="app">
    <MultiSelectParent />
  </div>
</template>

<script>
import MultiSelectParent from "./components/MultiSelectParent.vue";

export default {
  name: "App",

  components: {
    MultiSelectParent,
  },
};
</script>

<style>
body {
  padding: 0;
  margin: 0;
}
#app {
  font-family: "Roboto", helvetica, arial, sans-serif;
  text-align: center;
  color: #2c3e50;
  padding: 60px;
  display: flex;
  align-items: center;
  justify-content: center;
  flex-direction: column;
  position: relative;

  background: linear-gradient(
    135deg,
    rgba(65, 184, 131, 0.9),
    rgba(52, 73, 94, 0.9)
  );
  height: 100vh;
  font-size: 1.5em;
}

.header {
  color: #fff;
}

@media screen and (max-width: 480px) {
  #app {
    padding: 13px;
  }
}
</style>
