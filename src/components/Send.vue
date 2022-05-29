<script setup>
import { onMounted, ref } from "vue";

const emit = defineEmits(["view", "token"]);
let receiver = ref("");
let amount = ref("");
let message = ref(false);
let success = ref();
let users = ref();
let userArray = ref();

function send() {
  let data = {
    sender: localStorage.getItem("email"),
    receiver: receiver.value,
    amount: amount.value,
  };
  fetch("http://localhost:3001/api/v1/transactions/create", {
    method: "POST", // or 'PUT'
    headers: {
      "Content-Type": "application/json",
    },
    body: JSON.stringify(data),
  })
    .then((response) => response.json())
    .then((data) => {
      if (data != undefined) {
        if (data.status == "success") {
          emit("token", data.data);
          success.value = true;
        } else {
          success.value = false;
        }
        message.value = true;
      }
    })
    .catch((error) => {
      console.error(error);
    });
}

function getUsers() {
  let data = {token: localStorage.getItem("token")};
  fetch("http://localhost:3001/api/v1/users/all", {
    method: "POST", // or 'PUT'
    headers: {
      "Content-Type": "application/json",
    },
    body: JSON.stringify(data)
  })
    .then((response) => response.json())
    .then((data) => {
      userArray.value = Object.entries(data.data.users).map((arr) => ({
        email: arr[0],
        name: arr[1],
      }));
      
      userArray.value.forEach(element => {
        console.log(element.email);
      });
      users = data.data.users;
    })
    .catch((error) => {
      console.log(error);
    });
}

onMounted(() => {
  getUsers();
});
</script>

<template>
  <button @click="emit('view', 'home')" class="btn btn--small btn--top-left">
    <i class="fa-solid fa-arrow-left"></i>
  </button>
  <div>
    <h1>Send coins</h1>
    <!-- <input type="text" placeholder="Receiver's email" v-model="receiver" class="input"/> -->
    <select v-model="receiver" class="input" placeholder="receiver" required>
    <option value="" disabled selected>receiver</option>
      <option v-for="u in userArray" :value="u.email">
        {{ u.name }}
      </option>
    </select>    
    <input type="number" placeholder="Amount" v-model="amount" class="input" required/>
    <button @click="send" class="btn">Send</button>
    <div v-if="message" >
      <p v-if="success" class="message message--good">Transaction Sent.</p>
      <p v-if="!success" class="message message--bad">Something went wrong.</p>
      <button v-if="message" @click="emit('view', 'home')" class="btn">Back</button>
    </div>

  </div>
</template>

<style scoped>
div {
  display: flex;
  flex-direction: column;
}
</style>
