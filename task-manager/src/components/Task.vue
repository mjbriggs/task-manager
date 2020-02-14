<template>
  <div class='ui centered card'>
      
    <div class="content" >
      <div class='header'>
          {{ task.class }}
      </div>
      <div class='meta'>
          {{ task.name }}
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

