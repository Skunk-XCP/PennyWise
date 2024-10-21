<template>
  <div
    class="fixed inset-0 bg-gray-800 bg-opacity-75 flex items-center justify-center"
  >
    <div class="bg-white p-6 rounded-lg shadow-lg">
      <h2 class="text-xl font-bold mb-4">Ajouter une transaction</h2>
      <!-- Contenu de la modale : formulaire pour ajouter une transaction -->
      <p>Montant</p>
      <p>Catégorie</p>
      <p>Note</p>
      <button
        @click="$emit('close')"
        class="mt-4 p-2 bg-red-500 text-white rounded"
      >
        Fermer
      </button>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue'

export default defineComponent({
  name: 'TransactionModal',
})
</script>
