<template>
  <div id="app">
    <PlannerBoard/>
  </div>
</template>

<script>
import PlannerBoard from './components/PlannerBoard'

export default {
  name: 'app',
  components: {
    PlannerBoard
  }
}
</script>

<style>
body {
    font-family: Helvetica;
    color: #6b7588;
}

h1 {
  text-align: center;  
  text-transform: uppercase;
}

h2 {
  text-align: center;  
  text-transform: uppercase;
} 
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
}
</style>
