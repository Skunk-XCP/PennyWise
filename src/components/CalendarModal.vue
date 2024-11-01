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
        <!-- Jours du mois -->
        <div
          v-for="date in daysInMonth"
          :key="date"
          class="text-center p-4 border rounded-lg"
          :class="{ 'bg-blue-100': isToday(date) }"
        >
          {{ date }}
        </div>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { Icon } from '@iconify/vue'
import { computed, defineComponent } from 'vue'

export default defineComponent({
  name: 'CalendarModal',
  components: { Icon },
  props: {
    isOpen: {
      type: Boolean,
      required: true,
    },
  },
  emits: ['close'],
  setup(_, { emit }) {
    const now = new Date()
    const year = now.getFullYear()
    const month = now.getMonth()
    const today = now.getDate() // Jour actuel

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

    // Vérification manuelle si une date est aujourd'hui
    const isToday = (date: number) => {
      return date === today
    }

    return {
      days,
      daysInMonth,
      emptyDaysStart,
      isToday,
      closeModal,
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
