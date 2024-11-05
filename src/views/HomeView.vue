<template>
  <main>
    <FinanceCard :income="income" :expenses="expenses" />

    <TransactionModal
      v-if="showModal"
      @close="showModal = false"
      @transaction="handleTransaction"
      :initialMode="'income'"
      :selectedDate="selectedDate"
    />

    <!-- Affichage conditionnel -->
    <div
      v-if="transactions.length > 0"
      class="bg-zinc-200 h-96 mt-4 overflow-auto rounded-lg p-4 transaction-list-container"
      style="height: 70vh"
    >
      <CalendarModal
        v-if="isCalendarModalOpen"
        :isOpen="isCalendarModalOpen"
        :transactions="transactions"
        @close="isCalendarModalOpen = false"
      />

      <TransactionList :transactions="transactions" />
    </div>
    <div
      v-else
      class="bg-zinc-200 h-96 mt-4 flex items-center justify-center"
      style="height: 70vh"
    >
      <p class="text-zinc-400 text-xl font-bold text-center">No data</p>
    </div>

    <!-- Barre de tâches avec écoute de l'événement openCalendar -->
    <TaskBar @openModal="openModal" @openCalendar="openCalendarModal" />
  </main>
</template>

<script lang="ts">
import CalendarModal from '@/components/CalendarModal.vue'
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
    CalendarModal,
  },
  setup() {
    const income = ref(parseFloat(localStorage.getItem('income') || '0'))
    const expenses = ref(parseFloat(localStorage.getItem('expenses') || '0'))
    const transactions = ref<Transaction[]>([])
    const selectedDate = ref(new Date().toISOString().split('T')[0])

    const showModal = ref(false)
    const isCalendarModalOpen = ref(false)

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

    const openCalendarModal = () => {
      isCalendarModalOpen.value = true
    }

    const closeCalendarModal = () => {
      isCalendarModalOpen.value = false
    }

    const updateSelectedDate = (newDate: Date) => {
      selectedDate.value = newDate.toISOString().split('T')[0]
    }

    const handleTransaction = (transaction: {
      amount: number
      mode: 'income' | 'expenses'
      date: string
    }) => {
      if (transaction.mode === 'income') {
        income.value += transaction.amount
      } else if (transaction.mode === 'expenses') {
        expenses.value += transaction.amount
      }
      transactions.value.push(transaction)
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
      isCalendarModalOpen,
      openCalendarModal,
      closeCalendarModal,
      selectedDate,
      updateSelectedDate,
    }
  },
})
</script>

<style scoped>
.transaction-list-container {
  max-height: 70vh;
  overflow-y: auto;
}
.transaction-list-container::-webkit-scrollbar {
  width: 8px;
}
.transaction-list-container::-webkit-scrollbar-track {
  background: #f1f1f1;
  border-radius: 10px;
}
.transaction-list-container::-webkit-scrollbar-thumb {
  background: #c0c0c0;
  border-radius: 10px;
}
.transaction-list-container::-webkit-scrollbar-thumb:hover {
  background: #a0a0a0;
}
</style>
