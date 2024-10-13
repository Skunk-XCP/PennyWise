<template>
  <div class="finance-card flex justify-around">
    <FinanceCardItem
      class="bg-green-100 rounded-lg !border-0 p-5"
      label="Revenus"
      :value="income"
      valueColor="green"
    />
    <FinanceCardItem
      class="bg-red-100 rounded-lg !border-0 p-5"
      label="Dépenses"
      :value="expenses"
      valueColor="red"
    />
    <FinanceCardItem
      class="bg-gray-100 rounded-lg !border-0 p-5"
      label="Balance"
      :value="balance"
      valueColor="black"
    />
  </div>
</template>

<script lang="ts">
import { computed, defineComponent } from 'vue'
import FinanceCardItem from './FinanceCardItem.vue'

export default defineComponent({
  name: 'FinanceCard',
  components: {
    FinanceCardItem,
  },
  props: {
    income: {
      type: Number,
      required: true,
    },
    expenses: {
      type: Number,
      required: true,
    },
  },
  setup(props) {
    const balance = computed(() => props.income - props.expenses)

    return { balance }
  },
})
</script>
