<template>
  <div
    class="fixed inset-0 bg-gray-800 bg-opacity-75 flex items-center justify-center"
  >
    <div class="relative bg-white p-6 rounded-lg shadow-lg w-full max-w-md">
      <div class="flex justify-end mb-4">
        <button
          @click="$emit('close')"
          class="absolute top-2 right-2 p-2 rounded-full bg-gray-200 hover:bg-gray-300 focus:outline-none"
        >
          <Icon icon="mdi:close" class="h-5 w-5" />
        </button>
      </div>

      <!-- Navigation mois -->
      <div class="flex items-center justify-between mb-4">
        <button @click="previousMonth" class="text-gray-600 hover:text-black">
          <Icon icon="mdi:chevron-left" class="h-6 w-6" />
        </button>
        <h2 class="text-xl font-bold text-center">
          {{ monthName }} {{ currentYear }}
        </h2>
        <button @click="nextMonth" class="text-gray-600 hover:text-black">
          <Icon icon="mdi:chevron-right" class="h-6 w-6" />
        </button>
      </div>

      <!-- Calendrier -->
      <div class="grid grid-cols-7 gap-2">
        <!-- Affichage des jours de la semaine -->
        <div
          v-for="(day, index) in days"
          :key="index"
          class="text-center font-semibold"
        >
          {{ day }}
        </div>
        <!-- Espaces vides pour aligner les jours du mois -->
        <div
          v-for="empty in emptyDaysStart"
          :key="'empty' + empty"
          class="text-center p-4"
        ></div>
        <!-- Jours du mois -->
        <div
          v-for="day in daysInMonth"
          :key="day"
          class="text-center p-4 border rounded-lg cursor-pointer hover:bg-gray-200"
          :class="{ 'bg-green-200': isToday(day) }"
          @click="selectDate(day)"
        >
          {{ day }}
        </div>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { Icon } from '@iconify/vue'
import { computed, defineComponent, ref } from 'vue'

export default defineComponent({
  name: 'CalendarSimpleModal',
  components: { Icon },
  props: {
    selectedDate: {
      type: Date,
      required: true,
    },
  },
  emits: ['close', 'updateDate'],
  setup(props, { emit }) {
    const days = ['Lun', 'Mar', 'Mer', 'Jeu', 'Ven', 'Sam', 'Dim']

    // Variables pour le mois et l'année en cours
    const currentYear = ref(props.selectedDate.getFullYear())
    const currentMonth = ref(props.selectedDate.getMonth())

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

    // Calcul du nom du mois en cours
    const monthName = computed(() => monthNames[currentMonth.value])

    // Calcul du nombre de jours dans le mois
    const daysInMonth = computed(() => {
      const totalDays = new Date(
        currentYear.value,
        currentMonth.value + 1,
        0,
      ).getDate()
      return Array.from({ length: totalDays }, (_, i) => i + 1)
    })

    // Calcul des jours vides avant le début du mois
    const emptyDaysStart = computed(() => {
      const firstDay = new Date(
        currentYear.value,
        currentMonth.value,
        1,
      ).getDay()
      return firstDay === 0 ? 6 : firstDay - 1
    })

    // Sélection de la date
    const selectDate = (day: number) => {
      const newDate = new Date(currentYear.value, currentMonth.value, day)
      newDate.setHours(12, 0, 0, 0)
      emit('updateDate', newDate)
    }

    // Vérification si la date est aujourd'hui
    const today = new Date()
    const isToday = (day: number) => {
      return (
        today.getDate() === day &&
        today.getMonth() === currentMonth.value &&
        today.getFullYear() === currentYear.value
      )
    }

    // Navigation entre les mois
    const previousMonth = () => {
      if (currentMonth.value === 0) {
        currentMonth.value = 11
        currentYear.value -= 1
      } else {
        currentMonth.value -= 1
      }
    }

    const nextMonth = () => {
      if (currentMonth.value === 11) {
        currentMonth.value = 0
        currentYear.value += 1
      } else {
        currentMonth.value += 1
      }
    }

    return {
      days,
      daysInMonth,
      emptyDaysStart,
      selectDate,
      isToday,
      currentYear,
      monthName,
      previousMonth,
      nextMonth,
    }
  },
})
</script>
