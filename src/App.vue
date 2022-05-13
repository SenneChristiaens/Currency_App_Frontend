<script setup>
import { ref } from 'vue'
import LoginForm from './components/LoginForm.vue'
import RegisterForm from './components/Registerform.vue'

let viewLogin = ref(true);

function changeView() {
  viewLogin.value = !viewLogin.value;
}


</script>

<template>
  <LoginForm v-on:view="changeView" v-if="viewLogin"/>
  <RegisterForm v-on:view="changeView" v-if="!viewLogin"/>
</template>

<style>
#app {
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
