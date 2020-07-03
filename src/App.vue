<template>
  <TextEditor/>
</template>

<script>
// import HelloWorld from './components/HelloWorld.vue'
import TextEditor from "./components/TextEditor.vue"
export default {
  name: 'App',
  components: {
    // HelloWorld,
    TextEditor,
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
