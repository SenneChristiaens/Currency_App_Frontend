<script setup>
const emit = defineEmits(["view"]);
</script>

<template>
  <button @click="emit('view', 'home')" class="btn btn--small top-left">
    <i class="fa-solid fa-arrow-left"></i>
  </button>
  <div>
    <h1>Send coins</h1>
    <input type="text" placeholder="Receiver's email" v-model="receiver" class="input" />
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
