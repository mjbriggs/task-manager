<template>
 <div>
    <p>Tasks</p>
    <Task v-on:delete-task='deleteTask' v-on:task-complete='completeTask'
    v-for='task in Tasks' v-bind:key='task.id' v-bind:task='task'></Task>
  </div>
</template>

<script type = 'text/javascript' >
import Task from './Task'

export default {
  props: ['Tasks'],
  components: {
    Task
  },
  methods: {
    deleteTask (task) {
      const taskIndex = this.Tasks.indexOf(task)
      this.Tasks.splice(taskIndex, 1)
    },
    completeTask (task) {
      const taskIndex = this.Tasks.indexOf(task)
      this.Tasks[taskIndex].priority = 'completed'
      this.$emit('task-complete')
      // this.deleteTask(task)
    }
  }
}
</script>
<style>
</style>
