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

      <!-- Titre de la modal -->
      <h2 class="text-xl font-bold mb-4 text-center">Sélectionnez une date</h2>

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
import { computed, defineComponent } from 'vue'

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
    const year = props.selectedDate.getFullYear()
    const month = props.selectedDate.getMonth()

    const today = new Date()

    const daysInMonth = computed(() => {
      const totalDays = new Date(year, month + 1, 0).getDate()
      return Array.from({ length: totalDays }, (_, i) => i + 1)
    })

    const emptyDaysStart = computed(() => {
      const firstDay = new Date(year, month, 1).getDay()
      return firstDay === 0 ? 6 : firstDay - 1
    })

    const selectDate = (day: number) => {
      const newDate = new Date(year, month, day)
      emit('updateDate', newDate)
    }

    const isToday = (day: number) => {
      return (
        today.getDate() === day &&
        today.getMonth() === month &&
        today.getFullYear() === year
      )
    }

    return { days, daysInMonth, emptyDaysStart, selectDate, isToday }
  },
})
</script>
