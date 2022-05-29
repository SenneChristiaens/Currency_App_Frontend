<script setup>
import { ref } from "vue";

const emit = defineEmits(["view"]);
let email = ref("");
let pwd = ref("");
let message = ref(false);

function logIn() {
  let data = { email: email, password: pwd };
  fetch("http://localhost:3001/api/v1/users/login", {
    method: "POST", // or 'PUT'
    headers: {
      "Content-Type": "application/json",
    },
    body: JSON.stringify(data),
  })
    .then((response) => response.json())
    .then((data) => {
      if (data.data.token != undefined) {
        localStorage.setItem("token", data.data.token);
        emit("view", "loading");
      }
    })
    .catch((error) => {
      console.log("Invalid login credentials");
      message.value = true;
    });
}


</script>

<template>
  <div>
    <h1>Login</h1>
    <p class="message message--bad" v-if="message">Invalid login credentials</p>
    <input type="email" placeholder="E-mail" v-model="email" class="input" />
    <input type="password" placeholder="Password" v-model="pwd" class="input" />
    <button @click="logIn" class="btn">Log in</button>
    <button @click="$emit('view', 'register')" class="btn">Register</button>
  </div>
</template>

<style scoped>
div {
  display: flex;
  flex-direction: column;
}
</style>
