<template>
  <div id="app">
    <HelloWorld msg="Hello Vue in CodeSandbox!"/>
  </div>
</template>

<script>
import HelloWorld from "./components/HelloWorld";

export default {
  name: "App",
  components: {
    HelloWorld
  }
};
</script>

<style lang='scss'>
@import "./design/index";

body {
  font-family: Roboto;
  background-color: #282828;
  padding: 50px;
}
p {
  @extend %body-copy;
}
</style>
