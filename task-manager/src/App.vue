<template>
  <div id="app">
    
    <MainVue/>
  </div>
</template>

<script>
//import HelloWorld from './components/HelloWorld'
import MainVue from './components/MainVue'

export default {
  name: 'App',
  components: {
    //HelloWorld
    MainVue
  },

   // TODO: can we move it to the MainVue component?
    data(){
        return {
            /*
            format for a task
            {
                class: "cs356",
                name: "p1",
                priority: "high",
                dueDate: "too soon"
            }

            */
            Tasks: []
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
