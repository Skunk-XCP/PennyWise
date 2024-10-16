<script setup lang="ts">
import FinanceCard from '../components/FinanceCard.vue'
import TaskBar from '../components/TaskBar.vue'

const income = 5000
const expenses = 2000
</script>

<template>
  <main>
    <FinanceCard :income="income" :expenses="expenses" />
    <div
      class="bg-zinc-200 h-96 mt-4 flex items-center justify-center"
      style="height: 70vh"
    >
      <p class="text-zinc-400 text-xl font-bold text-center">No data</p>
    </div>
    <TaskBar />
  </main>
</template>
