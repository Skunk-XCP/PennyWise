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
import { defineComponent, onMounted, ref, watch } from 'vue'
import FinanceCard from '../components/FinanceCard.vue'
import TaskBar from '../components/TaskBar.vue'

interface Transaction {
  amount: number
  mode: 'income' | 'expenses'
  date: string
}

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
    const transactions = ref<Transaction[]>([])

    const showModal = ref(false)

    // Charger les transactions telles quelles, sans modifier les dates
    onMounted(() => {
      const storedTransactions = JSON.parse(
        localStorage.getItem('transactions') || '[]',
      )
      transactions.value = storedTransactions
    })

    const openModal = () => {
      showModal.value = true
    }

    const handleTransaction = (transaction: {
      amount: number
      mode: 'income' | 'expenses'
    }) => {
      if (transaction.mode === 'income') {
        income.value += transaction.amount
      } else if (transaction.mode === 'expenses') {
        expenses.value += transaction.amount
      }

      // Utiliser la date actuelle pour les nouvelles transactions, sans affecter les transactions existantes
      const todayDate = new Date().toISOString().split('T')[0]
      transactions.value.push({
        ...transaction,
        date: todayDate,
      })

      // Sauvegarder les transactions dans le localStorage sans altérer les dates d'origine
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
