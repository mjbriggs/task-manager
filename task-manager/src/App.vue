<template>
  <div id='app'>
    <AddTask v-on:add-task='addTask'/>
  </div>
</template>

<script>
import AddTask from './components/AddTask'

export default {
  name: 'App',
  components: {
    AddTask
  },

  // TODO: can we move it to the MainVue component?
  data () {
    return {
      /*
      format for a task
      {
        class: 'cs356',
        name: 'p1',
        priority: 'high',
        dueDate: 'too soon'
      }
      */
      Tasks: []
    }
  },
  methods: {
    addTask (task) {
      console.log('recieved task ' + task)
      this.Tasks.push(task)
      console.log('We have ' + this.Tasks.length + ' tasks')
    }
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
</style>
