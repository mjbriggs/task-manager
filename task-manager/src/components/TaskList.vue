<template>
  <div>

    <!-- DropDown for sorting -->
    <div class='field' id='sortDiv'>
      <label> Sort by: </label>
        <sui-dropdown id='dropDownBox'
          placeholder='priority'
          selection
          :options='sortOptions'
          v-model='sortOption'
          />
    </div>

    <!-- DropDown for filtering tasks -->
    <div class='field' id='filterDiv' >
      <label>Filter by: </label>
      <sui-dropdown id='dropDownBox'
        placeholder='All'
        selection
        :options='filterOptions'
        v-model='taskFilterOption'
        />
    </div>

     <!-- Search feature -->
     <div class='ui action input' id='searchDiv'>
      <label>Search in: </label>

      <sui-dropdown
        placeholder='Name'
        selection
        :options='searchOptions'
        v-model='searchOption'
        />

        <label> for: </label>
        <div class='field'>
          <input type='text' v-model='searchText' defaultValue />
        </div>
      
      
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
      sortOption: 'priority',
      filterOptions: [
        { text: 'All', value: 'All' },
        { text: 'High', value: 'High' },
        { text: 'Medium', value: 'Medium' },
        { text: 'Low', value: 'Low' },
        { text: 'Completed', value: 'completed' }
      ],
      searchOptions: [
        { text: 'Name', value: 'Name' },
        { text: 'Class', value: 'Class' }
      ],
      sortOptions: [
        { text: 'Priority', value: 'priority'},
        { text: 'Date', value: 'date'}
      ]
    }
  },
  watch: {
    taskFilterOption: function () {
      this.renderTasksOptions()
    },
    searchText: function () {
      this.renderTasksOptions()
    },
    sortOption: function () {
      this.renderTasksOptions()
    }
  },
  methods: {
    renderTasksOptions () { // Redupdate tasks to show
    console.log("rendering tasks to show")
      this.TasksToShow = this.Tasks
      this.filterTasksToShow()
      this.searchFilter()
      this.sortTasks()
    },
    filterTasksToShow () {
      console.log("filtering tasks")
      if (this.taskFilterOption === 'All') {
        this.TasksToShow = this.TasksToShow
      } else {
        this.TasksToShow = this.TasksToShow.filter(t => t.priority === this.taskFilterOption)
      }
    },
    searchFilter (){
      console.log("searching tasks")
      let subtext = this.searchText.toLowerCase()
      if(subtext === '') {
        return
      }
      else{
        if(this.searchOption === 'Name') {
          this.TasksToShow = this.TasksToShow.filter(t => t.name.toLowerCase().includes(subtext))
        }
        else if(this.searchOption === 'Class') {
          this.TasksToShow = this.TasksToShow.filter(t => t.class.toLowerCase().includes(subtext))
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
      this.renderTasksOptions()
    },
    undoComplete (task) {
      const taskIndex = this.Tasks.indexOf(task)
      this.Tasks[taskIndex].priority = task.oldPriority
      this.renderTasksOptions()
    },
    sortTasks () {
        console.log('sorting tasks')
        this.TasksToShow.sort((taskA, taskB) => {
          // first sort completed task
          if (taskA.priority === 'completed') return 1

          if (taskB.priority === 'completed') return -1

          if (this.sortOption === 'priority') {
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
        } else if (taskPriority === 'Medium') {
          return 1
        } else if (taskPriority === 'Low') {
          return 0
        } else {
          return -1
        }
    }
  },
  beforeMount () {
    this.renderTasksOptions()
  },
  
}

</script>

<style>
label{
  padding: 10px;
  font-size: 25px;
}
#filterDiv{
  padding-top:15px;
}
#searchDiv{
  padding-bottom: 5px;
  padding-top: 15px;
}
#dropDownBox{
  padding-left: 10px;
}
</style>
