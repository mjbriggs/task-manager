<template>
  <div id='app'>
    <AddTask v-on:add-task='addTask'/>

    <!-- RATIO button for sorting -->
    <input type="radio" id=" " name="gender" value="male">
    <label for="male">Male</label><br>
    <input type="radio" id="female" name="gender" value="female">
    <label for="female">Female</label><br>

    <TaskList v-bind:Tasks="Tasks"/>
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
      /*
      format for a task
      {
        class: 'cs356',
        name: 'p1',
        priority: 0,1,2
        dueDate: 'too soon'
      }
      */
      Tasks: []
    }
  },
  methods: {
    addTask (task) {
      console.log('received task ' + task.name + ', ' + task.class + ", priority: " + task.priority)
      this.Tasks.push(task)
      console.log('We have ' + this.Tasks.length + ' tasks')
      
      // sorting
      this.Tasks.sort((a, b) => {return this.comparePriority(a, b, false)});

    },
    compareDate(taskA, taskB, fromPriority){
      console.log("TESTING: in copmareDate" );
      console.log("TESING: " + taskA.dueDate + ", " + taskB.dueDate);
      let date = Date.parse(taskA.dueDate) - Date.parse(taskB.dueDate);
      
      if(date > 0){
        console.log("> 0")
        return 1;
      } 

      if(date < 0){
        console.log("< 0")
         return -1;
      }

      if(fromPriority) {
        console.log("in from");
        return 0;
      }

      return this.comparePriority(taskA, taskB, true);
    },
    comparePriority(taskA, taskB, fromDate){
      console.log("TESTING: in comparePriority" );
      let priorityA = this.getPriority(taskA.priority);
      let priorityB = this.getPriority(taskB.priority);

      if(priorityA > priorityB) return -1;
        
      if(priorityB > priorityA) return 1;

      console.log("TESTING: flag" );
      if(fromDate) return 0; // if date have already been compared then the tasks are the same
      
      console.log("TESTING: flag" );
      return this.compareDate(taskA, taskB, true);

    },
    getPriority(taskPriority){
      if(taskPriority === "High"){
        return 2;
      }
      if(taskPriority === "Medium"){
        return 1;
      }
      else{
        return 0;
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
