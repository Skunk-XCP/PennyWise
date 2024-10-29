<template>
  <div class="space-y-4">
    <div
      v-for="(transaction, index) in [...transactions].reverse()"
      :key="index"
      class="flex justify-between p-2 bg-white rounded-lg shadow-md"
    >
      <span class="text-lg font-semibold">
        {{ transaction.mode === 'income' ? 'Revenus' : 'Dépenses' }}
      </span>
      <span
        :class="
          transaction.mode === 'income' ? 'text-green-500' : 'text-red-500'
        "
        class="text-lg font-semibold"
      >
        {{ transaction.amount.toFixed(2) }} €
      </span>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent, PropType } from 'vue'

export default defineComponent({
  name: 'TransactionList',
  props: {
    transactions: {
      type: Array as PropType<
        { amount: number; mode: 'income' | 'expenses' }[]
      >,
      required: true,
    },
  },
})
</script>
