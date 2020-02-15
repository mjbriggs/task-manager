<template>
<div class='ui centered card'>
  <div class="content" >
    <div class='header'>
        Class: {{ task.class }},    Name: {{ task.name }},    Priority: {{ task.priority }}
    </div>
    <div class="header">
      Due Date: {{ task.dueDate }}
    </div>
    </div>
  <div class='ui bottom attached blue basic button' v-on:click="completeTask">
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
