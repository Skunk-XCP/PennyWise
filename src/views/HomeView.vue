<template>
  <main>
    <FinanceCard
      :transactions="transactions"
      :selectedMonth="selectedMonth"
      :selectedYear="selectedYear"
      @changeMonth="updateSelectedMonth"
    />

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

      <TransactionList
        :transactions="transactions"
        :selectedMonth="selectedMonth"
        :selectedYear="selectedYear"
      />
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

    // Variables pour le mois et l'année sélectionnés
    const selectedMonth = ref(new Date().getMonth())
    const selectedYear = ref(new Date().getFullYear())

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

    // Fonction pour mettre à jour le mois et l'année sélectionnés
    const updateSelectedMonth = (month: number, year: number) => {
      selectedMonth.value = month
      selectedYear.value = year
    }

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
      selectedMonth,
      selectedYear,
      updateSelectedDate,
      updateSelectedMonth,
    }
  },
})
</script>
