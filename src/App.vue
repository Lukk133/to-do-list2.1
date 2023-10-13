<template>
  <Navbar></Navbar>

  <TodoApp></TodoApp>

</template>

<script>
import TodoApp from './components/TodoApp.vue'
import Navbar from './components/Navbar.vue';



export default {
  name: 'App',
  components: {
    TodoApp,
    Navbar
  }
}
</script>