<script setup>
import { ref } from "vue";
import LoginForm from "./components/LoginForm.vue";
import RegisterForm from "./components/Registerform.vue";
import Home from "./components/Home.vue";
import Transactions from "./components/Transactions.vue";
import Send from "./components/Send.vue";
import Loading from "./components/Loading.vue";

let token = ref("");
let view = ref("");
const howm = ref();
let info = ref();

if (localStorage.getItem("token") != undefined) {
  view = ref("home");
  token.value = localStorage.getItem("token");
} else {
  view = ref("login");
}
if (view.value == "Home") {
  console.log('home');
  howm.value.getInfo();
}
function changeView(value) {
  view.value = value;
  getInfo();
}
function setToken(t) {
  token.value = t;
  localStorage.setItem("token", t);
  getInfo();
}

function getInfo() {
  let data = { token: localStorage.getItem("token") };
  fetch("http://localhost:3001/api/v1/users/token", {
    method: "POST", // or 'PUT'
    headers: {
      "Content-Type": "application/json",
    },
    body: JSON.stringify(data),
  })
    .then((response) => response.json())
    .then((data) => {
      data.data.balance = data.data.balance.toString();
      data.data.balance = data.data.balance.replace(/\B(?=(\d{3})+(?!\d))/g,".");

      info.value = data.data;
      localStorage.setItem("userId", data.data.id);
      localStorage.setItem("firstname", data.data.firstname);
      localStorage.setItem("lastname", data.data.lastname);
      localStorage.setItem("email", data.data.email);
      localStorage.setItem("balance", data.data.balance);
    })
    .catch((error) => {
    });
}
</script>

<template>
  <LoginForm @view="changeView" @token="setToken" v-if="view == 'login'" />
  <RegisterForm @view="changeView" v-if="view == 'register'" />
  <Loading v-if="view == 'loading'" @view="changeView"/>
  <Home @view="changeView" v-if="view == 'home'" ref="howm"/>
  <Transactions @view="changeView" v-if="view == 'transactions'" />
  <Send @view="changeView" @token="setToken" v-if="view =='send'"/>
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
