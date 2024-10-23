<template>
  <div class="flex items-center space-x-2 mb-6">
    <span :class="{ 'text-gray-400': !isIncome }">Revenus</span>
    <div
      class="relative w-16 h-8 rounded-full bg-gray-300 cursor-pointer"
      @click="toggleSwitch"
    >
      <div
        class="absolute top-1 w-6 h-6 bg-white rounded-full shadow-md transform transition-transform"
        :class="isIncome ? 'translate-x-1' : 'translate-x-8'"
      ></div>
    </div>
    <span :class="{ 'text-gray-400': isIncome }">Dépenses</span>
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

    return {
      isIncome,
      toggleSwitch,
    }
  },
})
</script>
