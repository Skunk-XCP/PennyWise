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

      <h2 class="text-xl font-bold mb-4">Ajouter une transaction</h2>
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
        />
      </div>
      <p>Catégorie</p>
      <p>Note</p>
    </div>
  </div>
</template>

<script lang="ts">
import { Icon } from '@iconify/vue'
import { defineComponent, onBeforeMount, onMounted } from 'vue'

export default defineComponent({
  name: 'TransactionModal',
  components: {
    Icon,
  },
  setup(props, { emit }) {
    const handleEscapeKey = (event: KeyboardEvent) => {
      if (event.key === 'Escape') {
        emit('close')
      }
    }

    onMounted(() => {
      window.addEventListener('keydown', handleEscapeKey)
    })

    onBeforeMount(() => {
      window.removeEventListener('keydown', handleEscapeKey)
    })
  },
})
</script>
