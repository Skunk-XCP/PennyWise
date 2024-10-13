<script setup lang="ts">
import FinanceCard from '../components/FinanceCard.vue'

const income = 5000
const expenses = 2000
</script>

<template>
  <main>
    <FinanceCard :income="income" :expenses="expenses" />
  </main>
</template>
