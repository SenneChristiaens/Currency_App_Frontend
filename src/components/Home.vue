<script setup>
import { ref } from 'vue';

const emit = defineEmits(['view']);
let logOutWarning = ref(false);
function onclik() {
    console.log("ls= " + localStorage.getItem("token"));
}
function logOut() {
    localStorage.clear();
    emit("view", "login");
}
</script>

<template>
  <div>
      <h1>Home</h1>
      <img src="../assets/eth.png" alt="IMDCOit" @click="onclik">
      <button @click="logOutWarning = !logOutWarning" class="btn">Log out</button>
  </div>
  <div class="warning" v-if="logOutWarning">
      <p>Do you really want to log out?</p>
      <button @click="logOut" class="btn">Yes</button>
      <button @click="logOutWarning = !logOutWarning" class="btn">No</button>
  </div>
</template>

<style scoped>
img {
    box-shadow: -10px -10px 20px rgba(255, 255, 255, 0.7), 10px 10px 20px rgba(189, 200, 223, 0.7);
    border-radius: 50%;
    max-width: 50%;
}
div {
    display: flex;
    flex-direction: column;
}
.warning {
    position: fixed;
    left: 0%;
    top: 0%;
    background-color: #E9EAF0;
    width: 100%;
    padding: 50% 0;
}
</style>
