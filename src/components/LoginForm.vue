<script setup>
import { ref } from 'vue';

const emit = defineEmits(['token']);
let email = ref("");
let pwd = ref("");

function logIn() {
  let data = { "email": email, "password": pwd };
  fetch('http://localhost:3001/api/v1/users/login', {
  method: 'POST', // or 'PUT'
  headers: {
    'Content-Type': 'application/json',
  },
  body: JSON.stringify(data)
})
.then(response => response.json())
.then(data => {
  emit('token', data.data.token);
})
.catch((error) => {
  console.error(error);
});
}

</script>

<template>
  <div>
      <h1>Login</h1>
      <input type="email" placeholder="E-mail" v-model="email" class="input">
      <input type="password" placeholder="Password" v-model="pwd" class="input">
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
