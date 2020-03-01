<template>
  <div id='app'>

    <h1>Sort My Tasks: University Edition</h1>

    <AddTask v-on:add-task='addTask' />

    <TaskList v-bind:Tasks='Tasks' ref='taskListRef'/>
  </div>
</template>

<script>
import AddTask from './components/AddTask'
import TaskList from './components/TaskList'

export default {
  name: 'App',
  components: {
    AddTask,
    TaskList
  },
  data () {
    return {
      Tasks: [],
    }
  },
  methods: {
    addTask (task) {
      console.log(
        'received task ' +
          task.name +
          ', ' +
          task.class +
          ', priority: ' +
          task.priority
      )
      this.Tasks.push(task)
      this.$refs.taskListRef.renderTasksOptions()
    },
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
h1{
  padding-top: 10px;
  font-size: 40px;
}
</style>
