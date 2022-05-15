<script setup>
import { ref } from "vue";

const emit = defineEmits(["view"]);

let firstName = ref("");
let lastName = ref("");
let email = ref("");
let pwd = ref("");

function Register() {
  let data = {
    firstname: firstName,
    lastname: lastName,
    email: email,
    password: pwd,
  };
  fetch("http://localhost:3001/api/v1/users", {
    method: "POST", // or 'PUT'
    headers: {
      "Content-Type": "application/json",
    },
    body: JSON.stringify(data),
  })
    .then((response) => response.json())
    .then((data) => {
      emit("view", "login");
    })
    .catch((error) => {
      console.error(error);
    });
}
</script>

<template>
  <div>
    <h1>Register</h1>
    <input
      type="text"
      placeholder="First name"
      v-model="firstName"
      class="input"
    />
    <input
      type="text"
      placeholder="Last name"
      v-model="lastName"
      class="input"
    />
    <input type="email" placeholder="E-mail" v-model="email" class="input" />
    <input type="password" placeholder="Password" v-model="pwd" class="input" />
    <button @click="Register" class="btn">Register</button>
    <button @click="$emit('view', 'login')" class="btn">
      Already have an account?
    </button>
  </div>
</template>

<style scoped>
div {
  display: flex;
  flex-direction: column;
}
</style>
