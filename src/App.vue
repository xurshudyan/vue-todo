<template>
   <div id="app" class="container">
      <img src="./assets/logo.png" alt="logo" class="logo">
      <TodoList/>
   </div>
</template>

<script>
   import TodoList from "./components/TodoList";

   export default {
      name: 'app',
      components: {TodoList}
   }
</script>

<style scoped>
   #app {
      font-family: 'Avenir', Helvetica, Arial, sans-serif;
      -webkit-font-smoothing: antialiased;
      -moz-osx-font-smoothing: grayscale;
      /* text-align: center; */
      color: #2c3e50;
      margin-top: 60px;
      font-size: 24px;
   }

   * {
      box-sizing: border-box;
   }

   .container {
      max-width: 600px;
      margin: 0 auto;
   }

   .logo {
      display: block;
      margin: 20px auto;
      height: 75px;
   }
</style>
