<template>
  <h1>{{ data.number }}</h1>
  <button @click="increment">Inrement</button>
  <button @click="() => data.number = 0">Reset</button>
  <Card :number="data.number" />
</template>

<script setup>
import { reactive } from 'vue';
import Card from './components/Card.vue'

  const data = reactive({})
  data.number = 0

  function increment(){
    data.number++
  }
</script>

<style scoped></style>