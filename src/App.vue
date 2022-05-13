<script setup>
import { ref } from 'vue'
import LoginForm from './components/LoginForm.vue'
import RegisterForm from './components/Registerform.vue'
import Home from './components/Home.vue'

let token = ref("");
let view = ref("");
if(localStorage.getItem("token") != undefined) {
  view = ref('home');
} else {
  view = ref('login');
}

function changeView(value) {
  view.value = value;
}
function setToken(t) {
  token.value = t;
  changeView("home");
  localStorage.setItem("token", t);
}


</script>

<template>
  <LoginForm @view="changeView" @token="setToken" v-if="view == 'login'"/>
  <RegisterForm @view="changeView" v-if="view == 'register'"/>
  <Home @view="changeView" v-if="view == 'home'"/>
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
