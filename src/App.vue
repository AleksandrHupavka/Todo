<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/logo.png">
    <todo-app />
  </div>
</template>
<script>

import  TodoApp from  "./components/TodoApp";

export default {
  components: {
    TodoApp
  }

};

</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2d4053;
  margin-top: 60px;
}
</style>
