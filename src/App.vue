<template>
  <div id="app">
    <RegistrationForm/>
  </div>
  
</template>

<script>

import RegistrationForm from './components/RegistrationForm.vue';

export default {
  name: 'App',
  components: {
    RegistrationForm
  }
}
</script>

<style>
:root {
    --main: #d6d6d6;
    --secondary: #6a5770;
    --accent: #4c375c;
    --text-color: #202020;
}


#app {
  font-family: Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;

  color: #2c3e50;
  margin-top: 60px;
}
</style>
