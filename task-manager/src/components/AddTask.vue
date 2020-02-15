<template lang='html'>
  <div>
    <!-- vue requires a main div for the component -->

    <h1>Sort My Tasks</h1>

    <div class='ui basic content center aligned segment'>
      <button class='ui basic button icon' v-on:click='toggleAdd'>
        <h2>
          Add a task
          <i class='plus icon' v-show='!isAdding' ></i>
          <i class='minus icon' v-show='isAdding' ></i>
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
            </div>
            <div class='field'>
                <label>Priority</label>
                <sui-dropdown
                  placeholder='Priority'
                  selection
                  :options='options'
                  v-model='priorityInput'
                />
              </div>
            <div class='field'>
                <label>Due Date</label>
                <input type='date' v-model='dueDateInput' defaultValue />
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
  data () {
    return {
      isAdding: false, // show adding task block and +/- icon
      classInput: '',
      taskInput: '',
      priorityInput: null,
      dueDateInput: '',
      options: [
        {
          text: 'Low',
          value: 'Low'
        },
        {
          text: 'Medium',
          value: 'Medium'
        },
        {
          text: 'High',
          value: 'High'
        }
      ]
    }
  },

  methods: {
    toggleAdd () {
      if (this.isAdding) {
        this.cancelAdd()
      } else {
        this.tryAdd()
      }
    },
    addTask () {
      if (this.isAdding) {
        if (this.taskInput.length > 0 &&
          this.priorityInput != null &&
          this.classInput.length > 0 &&
          this.dueDateInput.length > 0) {
          let dateFields = this.dueDateInput.split('-')
          let reformattedDate = dateFields[1] + '/' + dateFields[2] + '/' + dateFields[0]
          let newTask = {
            'class': this.classInput,
            'name': this.taskInput,
            'priority': this.priorityInput,
            'dueDate': reformattedDate
          }
          console.log('Emitting newTask')
          console.log('AddTask.addTask(): ' + newTask)
          this.$emit('add-task', newTask)

          this.taskInput = ''
          this.priorityInput = null
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
      this.taskInput = ''
      this.priorityInput = null
      this.dueDateInput = ''
      this.classInput = ''
      this.isAdding = false
    }
  }
}
</script>
<style></style>
