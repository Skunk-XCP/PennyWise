<template>
  <div class="space-y-6">
    <div v-for="(transactions, date) in groupedTransactions" :key="date">
      <div
        class="text-center bg-gray-100 p-2 rounded-lg font-bold text-lg mb-2"
      >
        {{ formatDate(date) }}
      </div>
      <div
        v-for="(transaction, index) in [...transactions].reverse()"
        :key="index"
        class="flex justify-between p-2 bg-white rounded-lg shadow-md mb-2"
      >
        <span class="text-lg font-semibold">
          {{ transaction.mode === 'income' ? 'Revenus' : 'Dépenses' }}
        </span>
        <span
          :class="
            transaction.mode === 'income' ? 'text-green-500' : 'text-red-500'
          "
          class="text-lg font-semibold"
        >
          {{ transaction.amount.toFixed(2) }} €
        </span>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { computed, defineComponent, PropType } from 'vue'

export default defineComponent({
  name: 'TransactionList',
  props: {
    transactions: {
      type: Array as PropType<
        { amount: number; mode: 'income' | 'expenses'; date: string }[]
      >,
      required: true,
    },
  },
  setup(props) {
    // Regroupe les transactions par date
    const groupedTransactions = computed(() => {
      // Trie d'abord les transactions par date décroissante
      const sortedTransactions = [...props.transactions].sort(
        (a, b) => new Date(b.date).getTime() - new Date(a.date).getTime(),
      )

      return sortedTransactions.reduce(
        (acc, transaction) => {
          const date = transaction.date
          if (!acc[date]) {
            acc[date] = []
          }
          acc[date].push(transaction)
          return acc
        },
        {} as Record<
          string,
          { amount: number; mode: 'income' | 'expenses'; date: string }[]
        >,
      )
    })

    // Formatage de la date
    const formatDate = (date: string) => {
      const today = new Date().toISOString().split('T')[0]
      if (date === today) return "Aujourd'hui"
      const options: Intl.DateTimeFormatOptions = {
        day: 'numeric',
        month: 'long',
        year: 'numeric',
      }
      return new Date(date).toLocaleDateString('fr-FR', options)
    }

    return {
      groupedTransactions,
      formatDate,
    }
  },
})
</script>
