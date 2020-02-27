<template>
  <div id='app'>
    <AddTask v-on:add-task='addTask' />

    <!-- RATIO button for sorting -->
    <div>
      <label for='one'>
        sort by: date
        <input type='radio' id='sortingType' value='date' v-model='sortByPriority' />
        priority
        <input
          type='radio'
          id='prioritySorting'
          value='priority'
          v-model='sortByPriority'
        />
      </label>
    </div>

    <TaskList v-bind:Tasks='Tasks' v-on:task-complete='sort' />
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
      sortByPriority: 'priority'
    }
  },
  watch: {
    sortByPriority: function () {
      this.sort()
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
      this.sort()
    },
    sort () {
      console.log('sorting by: ' + this.sortByPriority)
      this.Tasks.sort((taskA, taskB) => {
        // first sort completed task
        if (taskA.priority === 'completed') return 1

        if (taskB.priority === 'completed') return -1

        if (this.sortByPriority === 'priority') {
          return this.comparePriority(taskA, taskB, false)
        } else {
          return this.compareDate(taskA, taskB, false)
        }
      })
    },
    compareDate (taskA, taskB, fromPriority) {
      let date = Date.parse(taskA.dueDate) - Date.parse(taskB.dueDate)

      if (date > 0) return 1
      if (date < 0) return -1

      if (fromPriority) return 0
      return this.comparePriority(taskA, taskB, true)
    },
    comparePriority (taskA, taskB, fromDate) {
      let priorityA = this.getPriority(taskA.priority)
      let priorityB = this.getPriority(taskB.priority)

      if (priorityA > priorityB) return -1
      if (priorityB > priorityA) return 1

      if (fromDate) return 0 // if dates have already been compared then the tasks are the same
      return this.compareDate(taskA, taskB, true)
    },
    getPriority (taskPriority) {
      if (taskPriority === 'High') {
        return 2
      }
      if (taskPriority === 'Medium') {
        return 1
      } else {
        return 0
      }
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
