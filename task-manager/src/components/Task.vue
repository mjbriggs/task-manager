<template>
<div class='ui centered card '>
  <div class='content' >
    <div class='header'>
        <p> Class: {{ task.class }},    Name: {{ task.name }}</p>
        <p> Priority: {{ task.priority }} </p>
    </div>
    <div class="header">
      Due Date: {{ task.dueDate }}
    </div>
    </div>
  <div v-bind:class="colorBtn" v-on:click="handleTask">
      {{ this.buttonText }}
  </div>
</div>
</template>

<script>
export default {
  props: ['task'],
  data () {
    if (this.task.priority === 'High') {
      return {
        colorBtn: 'ui bottom attached basic button red ',
        buttonText: 'Complete'
      }
    } else if (this.task.priority === 'Medium') {
      return {
        colorBtn: 'ui bottom attached basic button yellow ',
        buttonText: 'Complete'
      }
    } else if (this.task.priority === 'Low') {
      return {
        colorBtn: 'ui bottom attached basic button green ',
        buttonText: 'Complete'
      }
    } else {
      return {
        colorBtn: 'ui bottom attached basic button grey',
        buttonText: 'Undo'
      }
    }
  },
  methods: {
    handleTask () {
      if (this.buttonText === 'Complete') {
        this.$emit('task-complete', this.task)
      } else {
        this.$emit('task-undo-complete', this.task)
      }
      if (this.task.priority === 'High') {
        this.colorBtn = 'ui bottom attached basic button red '
        this.buttonText = 'Complete'
      } else if (this.task.priority === 'Medium') {
        this.colorBtn = 'ui bottom attached basic button yellow '
        this.buttonText = 'Complete'
      } else if (this.task.priority === 'Low') {
        this.colorBtn = 'ui bottom attached basic button green '
        this.buttonText = 'Complete'
      } else {
        this.colorBtn = 'ui bottom attached basic button grey'
        this.buttonText = 'Undo'
      }
    }
  }
}
</script>
