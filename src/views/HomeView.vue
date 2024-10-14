<script setup lang="ts">
import FinanceCard from '../components/FinanceCard.vue'
import TaskBar from '../components/TaskBar.vue'

const income = 5000
const expenses = 2000
</script>

<template>
  <main>
    <FinanceCard :income="income" :expenses="expenses" />
    <TaskBar />
  </main>
</template>
