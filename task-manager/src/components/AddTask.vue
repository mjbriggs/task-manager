<template>
  <div>
    <!-- vue requires a main div for the component -->

    <h1>Sort My Tasks</h1>

    <div class='ui basic content center aligned segment'>
      <button class='ui basic button icon' v-on:click='addTask'>
        <h2>
          Add a task
          <i class='plus icon' v-show='!isAdding' v-on:click='tryAdd'></i>
          <i class='minus icon' v-show='isAdding' v-on:click='cancelAdd'></i>
        </h2>
      </button>
      <div class='ui centered card' v-show='isAdding'>
        <div class='content'>
          <div class='ui form'>
            <div class='ui action input'> <!-- place multiple div on the same line -->
              <div class='field'>
                <label>Class</label>
                <input type='text' v-model='classInput' defaultValue />
              </div>
              <div class='field'>
                <label>Task</label>
                <input type='text' v-model='taskInput' defaultValue />
              </div>
              <div class='field'>
                <label>Priority</label>
                <input type='text' v-model='priorityInput' defaultValue />
              </div>
            </div>
            <div class='field'>
                <label>Due Date</label>
                <input type='text' v-model='dueDateInput' defaultValue />
            </div>

            <div class='ui two button attached buttons'>
              <button class='ui basic blue button' v-on:click='addTask' >Create</button>
              <button class='ui basic red button' v-on:click='cancelAdd'>Cancel</button>
            </div>
          </div>
        </div>
      </div>
    </div>

    <!-- end of the main div -->
  </div>
</template>

<script type = 'text/javascript' >
export default {
  props: ['Tasks'],
  data () {
    return {
      isAdding: false, // used to show adding task block and +/- icon
      classInput: '',
      taskInput: '',
      priorityInput: '',
      dueDateInput: ''
      /*
        format for a task
        {
            class: 'cs356',
            name: 'p1',
            priority: 'high',
            dueDate: 'too soon'
        }
      */
    }
  },

  methods: {
    addTask () {
      if (this.isAdding) {
        console.log(this.taskInput.length)
        if (this.taskInput.length > 0 &&
          this.priorityInput.length > 0 &&
          this.classInput.length > 0 &&
          this.dueDateInput.length > 0) {
          let newTask = {
            'class': this.classInput,
            'name': this.taskInput,
            'priority': this.priorityInput,
            'dueDate': this.dueDateInput
          }
          console.log('Emitting newTask')
          console.log(newTask)
          this.$emit('add-task', newTask)

          this.taskInput = ''
          this.priorityInput = ''
          this.dueDateInput = ''
          this.classInput = ''
          this.isAdding = false
        }
      }
    },
    tryAdd () {
      this.isAdding = true
    },
    cancelAdd () {
      this.isAdding = false
    },
    showInput () {
      console.log(this.classInput + ', ' +
        this.taskInput + ', ' +
        this.priorityInput + ', ' +
        this.dueDateInput)
    }
  }
}
</script>
<style></style>
