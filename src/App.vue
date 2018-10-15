<template>
  <div id="app" class="container">
    <img alt="Vue logo" class="logo" src="./assets/logo.png">
    <TodoList />
  </div>
</template>

<script>
import HelloWorld from './components/HelloWorld.vue';
import TodoList from './components/TodoList.vue';

export default {
  name: 'app',
  components: {
    HelloWorld,
    TodoList
  }
}
</script>

<style>

* {
  box-sizing: border-box;
}

.container {
  max-width:600px;
  margin: 0 auto;
}

.logo {
  display: block;
  margin: 20px auto;
  height:75px;
}

#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  margin-top: 60px;
  font-size: 24px;
}
</style>
