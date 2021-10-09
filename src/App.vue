<template>
  <div id="app">
   <heading/>
  </div>
</template>

<script>
import heading from './components/heading.vue'

export default {
  name: 'App',
  components: {
    heading
  },
  data() {
    return{
      name: "Steve"
    }
  },
  created(){
    localStorage.setItem('name', this.name)
  }
}
</script>

<style>
/* #app {
  font-family: 'Roboto Condensed', sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
} */
</style>
