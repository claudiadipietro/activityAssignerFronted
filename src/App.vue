<template>
  <TasksVue/>
</template>

<script>
import TasksVue from './components/Tasks.vue'

export default {
  name: 'App',
  components: {
    TasksVue
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #512c8d;
  margin-top: 60px;
}
</style>
