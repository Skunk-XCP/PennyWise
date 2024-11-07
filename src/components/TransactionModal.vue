<template>
  <div
    class="fixed inset-0 bg-gray-800 bg-opacity-75 flex items-center justify-center"
  >
    <div class="relative bg-white p-6 rounded-lg shadow-lg w-full max-w-md">
      <!-- Bouton croix pour fermer -->
      <button
        @click="$emit('close')"
        class="absolute top-2 right-2 p-2 rounded-full bg-gray-200 hover:bg-gray-300 focus:outline-none"
      >
        <!-- L'icône de la croix -->
        <Icon icon="emojione-v1:cross-mark" class="h-3 w-3" />
      </button>

      <!-- Toggle switch "Revenus / Dépenses" -->
      <ToggleSwitch :initialIncome="mode === 'income'" @updateMode="setMode" />

      <!-- Contenu de la modale : formulaire pour ajouter une transaction -->
      <div class="flex">
        <label for="amount" class="text-lg font-medium text-gray-700"
          >Montant</label
        >
        <input
          id="amount"
          type="number"
          placeholder="Saisir un montant"
          min="0"
          step="0.01"
          class="border rounded ml-8 pl-2"
          v-model.number="amount"
        />
      </div>

      <!-- Ligne pour sélectionner la date -->
      <div class="flex items-center my-4">
        <label class="text-lg font-medium text-gray-700">Date</label>
        <button
          class="ml-16 text-green-500 underline"
          @click="openDateSelector"
        >
          {{ formattedDate }}
        </button>
      </div>

      <button
        :class="[
          'w-full',
          'p-2',
          'mt-6',
          'rounded',
          'text-white',
          'text-xl',
          mode === 'income' ? 'bg-green-500' : 'bg-orange-500',
        ]"
        @click="addTransaction"
      >
        Save
      </button>

      <!-- Modal de sélection de date simple -->
      <CalendarSimpleModal
        v-if="isDateSelectorOpen"
        :selectedDate="internalSelectedDate"
        @close="isDateSelectorOpen = false"
        @updateDate="updateDate"
      />
    </div>
  </div>
</template>

<script lang="ts">
import { Icon } from '@iconify/vue'
import { computed, defineComponent, ref } from 'vue'
import CalendarSimpleModal from './CalendarSimpleModal.vue'
import ToggleSwitch from './ToggleSwitch.vue'

export default defineComponent({
  name: 'TransactionModal',
  components: {
    ToggleSwitch,
    Icon,
    CalendarSimpleModal,
  },
  emits: ['transaction', 'close'],
  props: {
    initialMode: {
      type: String,
      default: 'income',
    },
    selectedDate: {
      type: String,
      required: true,
    },
  },
  setup(props, { emit }) {
    const amount = ref<number | null>(null)
    const mode = ref<'income' | 'expenses'>('expenses')
    const internalSelectedDate = ref(new Date(props.selectedDate))
    const isDateSelectorOpen = ref(false)

    const formattedDate = computed(() => {
      return internalSelectedDate.value.toLocaleDateString('fr-FR', {
        year: 'numeric',
        month: 'long',
        day: 'numeric',
      })
    })

    const setMode = (newMode: 'income' | 'expenses') => {
      mode.value = newMode
    }

    const openDateSelector = () => {
      isDateSelectorOpen.value = true
    }

    const updateDate = (newDate: Date) => {
      internalSelectedDate.value = newDate
      isDateSelectorOpen.value = false
    }

    const addTransaction = () => {
      if (amount.value !== null && amount.value > 0) {
        emit('transaction', {
          amount: amount.value,
          mode: mode.value,
          date: internalSelectedDate.value.toISOString().split('T')[0],
        })
        amount.value = null
        emit('close')
      }
    }

    return {
      amount,
      mode,
      setMode,
      addTransaction,
      internalSelectedDate,
      formattedDate,
      isDateSelectorOpen,
      openDateSelector,
      updateDate,
    }
  },
})
</script>
