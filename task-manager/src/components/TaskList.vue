<template>
  <div>
    <!-- DropDown for filtering tasks -->
    <div class='field'>
      <label>Filter by : </label>
      <sui-dropdown
        placeholder='All'
        selection
        :options='filterOptions'
        v-model='taskFilterOption'
        />
    </div>

     <!-- Search feature -->
     <div class='ui action input'>
      <label>Search </label>
        <div class='field'>
          <input type='text' v-model='searchText' defaultValue />
        </div>
      
      <label> in </label>
      <sui-dropdown
        placeholder='Name'
        selection
        :options='searchOptions'
        v-model='searchOption'
        />
    </div>

    <!-- show tasks -->
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
      taskFilterOption: 'All',
      searchOption: 'Name',
      searchText: '',
      filterOptions: [
        { text: 'All', value: 'All' },
        { text: 'High', value: 'High' },
        { text: 'Medium', value: 'Medium' },
        { text: 'Low', value: 'Low' },
        { text: 'Completed', value: 'Completed' }
      ],
      searchOptions: [
        { text: 'Name', value: 'Name' },
        { text: 'Class', value: 'Class' }
      ]
    }
  },
  watch: {
    taskFilterOption: function () {
      this.filterTasksToShow()
    },
    searchText: function() {
      this.searchFilter()
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
    searchFilter (){
      let subtext = this.searchText
      if(subtext === '') {
        this.filterTasksToShow()
      }
      else{
        if(this.searchOption === 'Name') {
          this.TasksToShow = this.TasksToShow.filter(t => t.name.includes(subtext))
        }
        else if(this.searchOption === 'Class') {
          this.TasksToShow = this.TasksToShow.filter(t => t.class.includes(subtext))
        }
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
