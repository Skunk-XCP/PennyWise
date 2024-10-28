<template>
  <div class="flex justify-center items-center space-x-4 mb-8">
    <!-- Switch container -->
    <div
      class="relative w-48 h-10 rounded-full bg-white border-2 border-gray-300 cursor-pointer flex items-center"
      @click="toggleSwitch"
    >
      <!-- Bouton coulissant -->
      <div
        class="absolute w-24 h-8 rounded-full shadow-md transform transition-transform"
        :class="{ 'bg-green-500': isIncome, 'bg-orange-500': !isIncome }"
        :style="{ left: isIncome ? '2px' : '90px' }"
      ></div>

      <!-- Texte "Revenus" et "Dépenses" -->
      <span
        class="absolute left-4 font-semibold transition-colors duration-300"
        :class="isIncome ? 'text-white' : 'text-gray-400'"
      >
        Revenus
      </span>
      <span
        class="absolute right-4 font-semibold transition-colors duration-300"
        :class="!isIncome ? 'text-white' : 'text-gray-400'"
      >
        Dépenses
      </span>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent, ref } from 'vue'

export default defineComponent({
  name: 'ToggleSwitch',
  props: {
    initialIncome: {
      type: Boolean,
      default: false,
    },
  },
  emits: ['updateMode'],
  setup(props, { emit }) {
    const isIncome = ref(props.initialIncome)

    const toggleSwitch = () => {
      isIncome.value = !isIncome.value
      emit('updateMode', isIncome.value ? 'income' : 'expenses')
    }

    return { isIncome, toggleSwitch }
  },
})
</script>
