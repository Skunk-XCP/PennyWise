<template>
  <main>
    <FinanceCard :income="income" :expenses="expenses" />

    <TransactionModal v-if="showModal" @close="showModal = false" />

    <div
      class="bg-zinc-200 h-96 mt-4 flex items-center justify-center"
      style="height: 70vh"
    >
      <p class="text-zinc-400 text-xl font-bold text-center">No data</p>
    </div>
    <TaskBar @openModal="openModal" />
  </main>
</template>

<script lang="ts">
import TransactionModal from '@/components/TransactionModal.vue'
import { defineComponent, ref } from 'vue'
import FinanceCard from '../components/FinanceCard.vue'
import TaskBar from '../components/TaskBar.vue'

export default defineComponent({
  components: {
    FinanceCard,
    TaskBar,
    TransactionModal,
  },
  setup() {
    const income = 5000
    const expenses = 2000

    const showModal = ref(false)

    const openModal = () => {
      showModal.value = true
    }

    return { showModal, openModal, income, expenses }
  },
})
</script>
