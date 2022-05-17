<script setup>
import { ref } from "vue";

const emit = defineEmits(["view"]);
let receiver = ref("");
let amount = ref("");

function Send() {
  let data = {
    receiver: receiver,
    amount: amount,
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
        emit("view", "transactions");
      }
    })
    .catch((error) => {
      console.error(error);
    });
}
</script>

<template>
  <button @click="emit('view', 'home')" class="btn btn--small top-left">
    <i class="fa-solid fa-arrow-left"></i>
  </button>
  <div>
    <h1>Send coins</h1>
    <input
      type="text"
      placeholder="Receiver's email"
      v-model="receiver"
      class="input"
    />
    <input type="number" placeholder="Amount" v-model="amount" class="input" />
    <button @click="Send" class="btn">Send</button>
  </div>
</template>

<style scoped>
div {
  display: flex;
  flex-direction: column;
}
</style>
