<script setup>
import { onMounted, ref } from "vue";

const emit = defineEmits(["view"]);
const delay = ms => new Promise(res => setTimeout(res, ms));
const load = async () => {
  await delay(3000);
  emit("view", "home");

};

onMounted(() => {
  load();
});
</script>

<template>
  <div>
    <h1>Loading...</h1>
  </div>
</template>

<style scoped>
div {
  display: flex;
  flex-direction: column;
}
</style>
