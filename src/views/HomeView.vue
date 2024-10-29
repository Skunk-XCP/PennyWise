<template>
  <main>
    <FinanceCard :income="income" :expenses="expenses" />

    <TransactionModal
      v-if="showModal"
      @close="showModal = false"
      @transaction="handleTransaction"
      :initialMode="'income'"
    />

    <!-- Affichage conditionnel -->
    <div
      v-if="transactions.length > 0"
      class="bg-zinc-200 h-96 mt-4 overflow-auto rounded-lg p-4"
      style="height: 70vh"
    >
      <TransactionList :transactions="transactions" />
    </div>
    <div
      v-else
      class="bg-zinc-200 h-96 mt-4 flex items-center justify-center"
      style="height: 70vh"
    >
      <p class="text-zinc-400 text-xl font-bold text-center">No data</p>
    </div>
    <TaskBar @openModal="openModal" />
  </main>
</template>

<script lang="ts">
import TransactionList from '@/components/TransactionList.vue'
import TransactionModal from '@/components/TransactionModal.vue'
import { defineComponent, ref, watch } from 'vue'
import FinanceCard from '../components/FinanceCard.vue'
import TaskBar from '../components/TaskBar.vue'

export default defineComponent({
  components: {
    FinanceCard,
    TaskBar,
    TransactionModal,
    TransactionList,
  },
  setup() {
    const income = ref(parseFloat(localStorage.getItem('income') || '0'))
    const expenses = ref(parseFloat(localStorage.getItem('expenses') || '0'))
    const transactions = ref<{ amount: number; mode: 'income' | 'expenses' }[]>(
      JSON.parse(localStorage.getItem('transactions') || '[]'),
    )

    const showModal = ref(false)

    const openModal = () => {
      showModal.value = true
    }

    const handleTransaction = (transaction: {
      amount: number
      mode: 'income' | 'expenses'
    }) => {
      // Ajouter la transaction aux revenus ou dépenses
      if (transaction.mode === 'income') {
        income.value += transaction.amount
      } else if (transaction.mode === 'expenses') {
        expenses.value += transaction.amount
      }

      // Ajouter la transaction à la liste
      transactions.value.push(transaction)

      // Sauvegarder les transactions dans le localStorage
      localStorage.setItem('transactions', JSON.stringify(transactions.value))
    }

    watch(income, newIncome => {
      localStorage.setItem('income', newIncome.toString())
    })

    watch(expenses, newExpenses => {
      localStorage.setItem('expenses', newExpenses.toString())
    })

    return {
      showModal,
      openModal,
      income,
      expenses,
      transactions,
      handleTransaction,
    }
  },
})
</script>
