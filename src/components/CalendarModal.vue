<template>
  <div
    v-if="isOpen"
    class="fixed inset-0 bg-gray-800 bg-opacity-75 flex items-center justify-center"
  >
    <div class="relative bg-white p-6 rounded-lg shadow-lg w-full max-w-md">
      <button
        @click="closeModal"
        class="absolute top-2 right-2 p-2 rounded-full bg-gray-200 hover:bg-gray-300"
      >
        <Icon icon="mdi:close" class="h-5 w-5" />
      </button>
      <h2 class="text-xl font-bold mb-4 text-center">Calendrier</h2>
      <div class="grid grid-cols-7 gap-2">
        <!-- En-têtes des jours de la semaine -->
        <div
          v-for="(day, index) in days"
          :key="index"
          class="text-center font-semibold"
        >
          {{ day }}
        </div>
        <!-- Espaces vides avant le début du mois -->
        <div
          v-for="empty in emptyDaysStart"
          :key="'empty' + empty"
          class="text-center p-4"
        ></div>
        <!-- Jours du mois avec transactions -->
        <div
          v-for="date in daysInMonth"
          :key="date"
          class="p-2 border rounded-lg flex flex-col items-center justify-start h-20 overflow-hidden relative"
          :class="{ 'bg-blue-100': isToday(date) }"
        >
          <span class="text-sm font-semibold">{{ date }}</span>
          <!-- Afficher les transactions limitées à un certain nombre de lignes -->
          <div class="overflow-hidden max-h-10 text-center">
            <div
              v-for="transaction in (
                transactionsByDate[formatDate(year, month, date)] || []
              ).slice(0, 2)"
              :key="transaction.amount"
              class="text-xs font-semibold truncate"
              :class="
                transaction.mode === 'income'
                  ? 'text-green-500'
                  : 'text-red-500'
              "
            >
              {{ transaction.amount }} €
            </div>
          </div>
          <!-- Indicateur si plus de transactions existent -->
          <span
            v-if="
              (transactionsByDate[formatDate(year, month, date)] || []).length >
              3
            "
            class="absolute bottom-1 text-xs text-gray-500"
          >
            ...
          </span>
        </div>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { Icon } from '@iconify/vue'
import { computed, defineComponent, PropType } from 'vue'

interface Transaction {
  amount: number
  mode: 'income' | 'expenses'
  date: string
}

export default defineComponent({
  name: 'CalendarModal',
  components: { Icon },
  props: {
    isOpen: {
      type: Boolean,
      required: true,
    },
    transactions: {
      type: Array as PropType<Transaction[]>,
      required: true,
    },
  },
  emits: ['close'],
  setup(props, { emit }) {
    const now = new Date()
    const year = now.getFullYear()
    const month = now.getMonth()
    const today = now.getDate()

    const days = ['Lun', 'Mar', 'Mer', 'Jeu', 'Ven', 'Sam', 'Dim']

    // Création de chaque jour du mois comme simple numéro
    const daysInMonth = computed(() => {
      const totalDaysInMonth = new Date(year, month + 1, 0).getDate()
      return Array.from({ length: totalDaysInMonth }, (_, i) => i + 1)
    })

    // Calcul des jours vides avant le début du mois
    const emptyDaysStart = computed(() => {
      const firstDayOfMonth = new Date(year, month, 1).getDay()
      return firstDayOfMonth === 0 ? 6 : firstDayOfMonth - 1
    })

    // Fonction pour fermer la modal
    const closeModal = () => emit('close')

    // Vérification si une date est aujourd'hui
    const isToday = (date: number) => {
      return date === today
    }

    // Formatage de la date pour correspondre à celles des transactions
    const formatDate = (year: number, month: number, day: number) => {
      const monthString = (month + 1).toString().padStart(2, '0')
      const dayString = day.toString().padStart(2, '0')
      return `${year}-${monthString}-${dayString}`
    }

    // Grouper les transactions par date
    const transactionsByDate = computed(() => {
      const grouped: Record<string, Transaction[]> = {}
      props.transactions.forEach(transaction => {
        const transactionDate = transaction.date
        if (!grouped[transactionDate]) {
          grouped[transactionDate] = []
        }
        grouped[transactionDate].push(transaction)
      })
      return grouped
    })

    return {
      days,
      daysInMonth,
      emptyDaysStart,
      isToday,
      formatDate,
      transactionsByDate,
      closeModal,
      year,
      month,
      today,
    }
  },
})
</script>

<style scoped>
.grid-cols-7 {
  display: grid;
  grid-template-columns: repeat(7, 1fr);
}
</style>
