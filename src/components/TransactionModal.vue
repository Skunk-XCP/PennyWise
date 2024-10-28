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
          class="border rounded ml-8"
          v-model.number="amount"
        />
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
    </div>
  </div>
</template>

<script lang="ts">
import { Icon } from '@iconify/vue'
import { defineComponent, ref } from 'vue'
import ToggleSwitch from './ToggleSwitch.vue'

export default defineComponent({
  name: 'TransactionModal',
  components: {
    ToggleSwitch,
    Icon,
  },
  emits: ['transaction', 'close'],
  props: {
    initialMode: {
      type: String,
      default: 'income',
    },
  },
  setup(props, { emit }) {
    const amount = ref<number | null>(null)
    const mode = ref<'income' | 'expenses'>('expenses')

    const setMode = (newMode: 'income' | 'expenses') => {
      mode.value = newMode
    }

    const addTransaction = () => {
      if (amount.value !== null && amount.value > 0) {
        emit('transaction', {
          amount: amount.value,
          mode: mode.value,
        })
        amount.value = null
        emit('close')
      }
    }

    return { amount, mode, setMode, addTransaction }
  },
})
</script>
