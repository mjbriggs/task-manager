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
    <div class='ui bottom attached green basic button' >
        Completed
    </div>
  </div>
</template>

<script>
export default {
    props: ['task'],
    method: {
        completeTask(){}
    }
}

</script>

