<template>
  <div>
    <!-- DropDown for filtering tasks -->
    <div class='field'>
      <label>Filter by</label>
      <sui-dropdown
        placeholder='All'
        selection
        :options='filterOptions'
        v-model='taskFilterOption'
        />
    </div>

    <!-- show tasks -->
    <p>Tasks</p>
    <Task
      v-on:delete-task="deleteTask"
      v-on:task-complete="completeTask"
      v-on:task-undo-complete="undoComplete"
      v-for="task in TasksToShow"
      v-bind:key="task.id"
      v-bind:task="task"
    ></Task>
    
  </div>
</template>

<script type = 'text/javascript' >
import Task from "./Task";

export default {
  props: ["Tasks"],
  components: {
    Task
  },
  data () {
    return {
      TasksToShow: [],
      taskFilterOption: "All",
      filterOptions: [
        { text: 'All', value: 'All' },
        { text: 'High', value: 'High' },
        { text: 'Medium', value: 'Medium' },
        { text: 'Low', value: 'Low' },
        { text: 'Completed', value: 'Completed' }
      ]
    }
  },
  watch: {
    taskFilterOption: function () {
      this.filterTasksToShow()
    }
  },
  methods: {
    filterTasksToShow () {
      console.log(this.taskFilterOption);
      if (this.taskFilterOption === 'All') {
        this.TasksToShow = this.Tasks
      } else {
        this.TasksToShow = this.Tasks.filter(t => t.priority === this.taskFilterOption)
      }
    },
    deleteTask (task) {
      const taskIndex = this.Tasks.indexOf(task)
      this.Tasks.splice(taskIndex, 1)
    },
    completeTask (task) {
      const taskIndex = this.Tasks.indexOf(task)
      this.Tasks[taskIndex].priority = 'completed'
      this.$emit('task-complete');
      // this.deleteTask(task)
    },
    undoComplete (task) {
      const taskIndex = this.Tasks.indexOf(task)
      this.Tasks[taskIndex].priority = task.oldPriority
      this.$emit('task-undo-complete')
      this.filterTasksToShow()
    }
  },
  beforeMount () {
    this.filterTasksToShow()
  }
};
</script>
<style>
</style>
