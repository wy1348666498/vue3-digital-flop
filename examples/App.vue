<template>
  <VueDigitalFlop :end="num" />
</template>

<script setup>
import { VueDigitalFlop } from "../packages/index.js";
import { ref } from "vue";

const num = ref(0);
setInterval(() => {
  num.value = num.value + Math.floor(Math.random() * 100);
}, 2500);
</script>

<style scoped></style>
