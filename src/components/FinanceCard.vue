<template>
  <div class="finance-card flex flex-col">
    <!-- Navigation des mois -->
    <div class="flex justify-center mb-6">
      <button @click="previousMonth" class="text-gray-600 hover:text-black">
        <Icon icon="mdi:chevron-left" class="h-6 w-6" />
      </button>
      <h2 class="text-xl font-bold text-center px-6">
        {{ monthName }} {{ selectedYear }}
      </h2>
      <button @click="nextMonth" class="text-gray-600 hover:text-black">
        <Icon icon="mdi:chevron-right" class="h-6 w-6" />
      </button>
    </div>

    <div class="flex justify-around mb-6">
      <FinanceCardItem
        class="bg-green-100 rounded-lg !border-0 p-5"
        label="Revenus"
        :value="monthlyIncome"
        valueColor="green"
      />
      <FinanceCardItem
        class="bg-red-100 rounded-lg !border-0 p-5"
        label="Dépenses"
        :value="monthlyExpenses"
        valueColor="red"
      />
      <FinanceCardItem
        class="bg-gray-100 rounded-lg !border-0 p-5"
        label="Balance"
        :value="monthlyBalance"
        valueColor="black"
      />
    </div>
  </div>
</template>

<script lang="ts">
import { Icon } from '@iconify/vue'
import { computed, defineComponent, PropType } from 'vue'
import FinanceCardItem from './FinanceCardItem.vue'

export default defineComponent({
  name: 'FinanceCard',
  components: {
    FinanceCardItem,
    Icon,
  },
  props: {
    transactions: {
      type: Array as PropType<
        { amount: number; mode: 'income' | 'expenses'; date: string }[]
      >,
      required: true,
    },
    selectedMonth: {
      type: Number,
      required: true,
    },
    selectedYear: {
      type: Number,
      required: true,
    },
  },
  emits: ['changeMonth'],
  setup(props, { emit }) {
    // Noms des mois en français
    const monthNames = [
      'Janvier',
      'Février',
      'Mars',
      'Avril',
      'Mai',
      'Juin',
      'Juillet',
      'Août',
      'Septembre',
      'Octobre',
      'Novembre',
      'Décembre',
    ]

    const monthName = computed(() => monthNames[props.selectedMonth])

    // Filtrer les transactions pour le mois et l'année sélectionnés
    const filteredTransactions = computed(() =>
      props.transactions.filter(transaction => {
        const transactionDate = new Date(transaction.date)
        return (
          transactionDate.getMonth() === props.selectedMonth &&
          transactionDate.getFullYear() === props.selectedYear
        )
      }),
    )

    // Calcul des revenus et dépenses mensuels
    const monthlyIncome = computed(() =>
      filteredTransactions.value
        .filter(transaction => transaction.mode === 'income')
        .reduce((sum, transaction) => sum + transaction.amount, 0),
    )

    const monthlyExpenses = computed(() =>
      filteredTransactions.value
        .filter(transaction => transaction.mode === 'expenses')
        .reduce((sum, transaction) => sum + transaction.amount, 0),
    )

    // Calcul de la balance mensuelle
    const monthlyBalance = computed(
      () => monthlyIncome.value - monthlyExpenses.value,
    )

    // Navigation de mois
    const previousMonth = () => {
      if (props.selectedMonth === 0) {
        emit('changeMonth', 11, props.selectedYear - 1)
      } else {
        emit('changeMonth', props.selectedMonth - 1, props.selectedYear)
      }
    }

    const nextMonth = () => {
      if (props.selectedMonth === 11) {
        emit('changeMonth', 0, props.selectedYear + 1)
      } else {
        emit('changeMonth', props.selectedMonth + 1, props.selectedYear)
      }
    }

    return {
      monthName,
      monthlyIncome,
      monthlyExpenses,
      monthlyBalance,
      previousMonth,
      nextMonth,
    }
  },
})
</script>
