<template>
<div class='ui centered card'>
  <div class="content" >
    <div class='header'>
        <p> Class: {{ task.class }},    Name: {{ task.name }}</p>
        <p> Priority: {{ task.priority }} </p>
    </div>
    <div class="header">
      Due Date: {{ task.dueDate }}
    </div>
    </div>
  <div class='ui bottom attached blue basic button' v-on:click="completeTask"  v-show='task.priority != "completed"'>
      Complete
  </div>
</div>
</template>

<script>
export default {
  props: ['task'],
  methods: {
    completeTask () {
      this.$emit('task-complete', this.task)
    }
  }
}
</script>
