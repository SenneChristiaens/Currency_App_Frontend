<script setup>
import { ref } from "vue";
import LoginForm from "./components/LoginForm.vue";
import RegisterForm from "./components/Registerform.vue";
import Home from "./components/Home.vue";
import Transactions from "./components/Transactions.vue";

let token = ref("");
let view = ref("");
if (localStorage.getItem("token") != undefined) {
  view = ref("home");
  token.value = localStorage.getItem("token");
} else {
  view = ref("login");
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
  <LoginForm @view="changeView" @token="setToken" v-if="view == 'login'" />
  <RegisterForm @view="changeView" v-if="view == 'register'" />
  <Home @view="changeView" v-if="view == 'home'" />
  <Transactions @view="changeView" v-if="view == 'transactions'" />
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
