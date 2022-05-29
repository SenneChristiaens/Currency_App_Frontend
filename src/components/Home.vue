<script setup>
import { ref, onMounted } from "vue";

const emit = defineEmits(["view"]);
let logOutWarning = ref(false);
let firstName = ref(localStorage.getItem('firstname'));
let balance = ref(localStorage.getItem('balance'));

function logOut() {
  localStorage.clear();
  emit("view", "login");
}
  
</script>

<template>
  <div class="main">
    <h1>Hello {{ firstName }}!</h1>
    <img src="../assets/eth.png" alt="IMDCoin" />
    <p>Your balance: {{ balance }} IC</p>
    <div class="flexrow">
      <button @click="$emit('view', 'send')" class="btn btn--small">
        <i class="fa-solid fa-arrow-right-from-bracket"></i>
      </button>
      <button @click="$emit('view', 'transactions')" class="btn btn--small">
        <i class="fa-solid fa-arrow-right-arrow-left"></i>
      </button>
    </div>
  </div>

  <button @click="logOutWarning = !logOutWarning" class="btn logout">
    Log out
  </button>
  <div class="warning" v-if="logOutWarning">
    <p>Do you really want to log out?</p>
    <button @click="logOut" class="btn">Yes</button>
    <button @click="logOutWarning = !logOutWarning" class="btn">No</button>
  </div>
</template>

<style scoped>
img {
  box-shadow: -10px -10px 20px rgba(255, 255, 255, 0.7),
    10px 10px 20px rgba(189, 200, 223, 0.7);
  border-radius: 50%;
  max-width: 50%;
  margin: 10%;
}
div {
  display: flex;
  flex-direction: column;
}
.main {
  justify-content: center;
  align-items: center;
  height: 90vh;
  overflow: hidden;
}
.logout {
  position: fixed;
  top: 10px;
  right: 0;
}
.flexrow {
  display: flex;
  flex-direction: row;
  width: 75%;
  justify-content: space-evenly;
  margin: 10% 0;
}
</style>
