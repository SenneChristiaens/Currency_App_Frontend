<script setup>
import { ref, onMounted, computed } from "vue";

const emit = defineEmits(["view"]);
let transactions = ref([]);
let email = ref(localStorage.getItem("email"));
let names = ref([]);

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
        data.data.transactions.forEach((t) => {
          // t.sender = getNameByMail(t.sender);
          // t.receiver = getNameByMail(t.receiver);
          if (t.receiver != email.value) {
            getNameByMail(t.receiver);
            t.receiver = names.value[names.value.length];
          } else if (t.sender != email.value) {
            getNameByMail(t.sender);
            t.sender = names.value[names.value.length];
          }
        });
        transactions.value = data.data.transactions;
      }
    })
    .catch((error) => {
      console.log(error);
    });
}

function getNameByMail(mail) {
  let data = { token: localStorage.getItem("token"), email: mail };
  fetch("http://localhost:3001/api/v1/users/email", {
    method: "POST", // or 'PUT'
    headers: {
      "Content-Type": "application/json",
    },
    body: JSON.stringify(data),
  })
    .then((response) => response.json())
    .then((data) => {
      if (data != undefined) {
        names.value.push(data.data.firstname + " " + data.data.lastname);
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
        <div v-if="t.sender == email.value" class="listitem">
          <i class="fa-solid fa-arrow-right-to-bracket green"></i>
          <span>From {{ names[i] }}</span>
          <span>{{ t.amount }} IC</span>
        </div>

        <div v-if="t.receiver == email.value" class="listitem">
          <i class="fa-solid fa-arrow-right-from-bracket red"></i>
          <span>To {{ names[i] }}</span>
          <span>{{ t.amount }} IC</span>
        </div>
      </li>
    </ul>
  </div>
  <button @click="emit('view', 'home')" class="btn btn--small btn--top-left">
    <i class="fa-solid fa-arrow-left"></i>
  </button>
</template>

<style scoped>
.flex {
  display: flex;
  flex-direction: column;
}

</style>
