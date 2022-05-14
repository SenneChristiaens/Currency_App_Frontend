<script setup>
import { ref, onMounted } from "vue";

const emit = defineEmits(["view"]);
let transactions = ref([]);
let firstname = ref(localStorage.getItem("firstname"));

function getTransactions() {
  let data = { token: localStorage.getItem("token") };
  fetch("http://localhost:3001/api/v1/transactions/", {
    method: "POST", // or 'PUT'
    headers: {
      "Content-Type": "application/json",
    },
    body: JSON.stringify(data),
  })
    .then((response) => response.json())
    .then((data) => {
      if (data != undefined) {
        //        emit('token', data.data.token);
        transactions.value = data.data.transactions;
      }
    })
    .catch((error) => {
      console.log(error);
    });
}

onMounted(() => {
  getTransactions();
});
</script>

<template>
  <div class="flex">
    <h1>Transactions</h1>
    <ul>
      <li v-for="(t, i) in transactions">

        <div v-if="t.receiver == firstname" class="listitem">
            <i class="fa-solid fa-arrow-right-to-bracket green"></i>
            <span>From {{ t.sender}}</span>
            <span>{{ t.amount}} IC</span>
        </div>

        <div v-if="t.sender == firstname" class="listitem">
            <i class="fa-solid fa-arrow-right-from-bracket red"></i>
            <span>To {{ t.receiver}}</span>
            <span>{{ t.amount}} IC</span>
        </div>
          

      </li>
    </ul>
  </div>
  <button @click="emit('view', 'home')" class="btn btn--small top-left">
    <i class="fa-solid fa-arrow-left"></i>
  </button>
</template>

<style scoped>
.flex {
  display: flex;
  flex-direction: column;
}
.top-left {
  position: fixed;
  top: 0;
  left: 0;
}
</style>
