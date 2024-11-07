<template>
  <div id="app">
    <RegistrationForm />
  </div>
</template>

<script>
import RegistrationForm from './components/RegistrationForm.vue';

export default {
  components: {
    RegistrationForm
  }
}
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: Arial, sans-serif;
  background-color: #f4f4f9;
  color: #333;
}

#app {
  display: flex;
  justify-content: center;
  align-items: center;
  min-height: 100vh;
}

h2 {
  margin-bottom: 20px;
}

.form {
  display: flex;
  flex-direction: column;
}
</style>
