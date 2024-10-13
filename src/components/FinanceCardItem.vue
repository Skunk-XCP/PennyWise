<template>
  <div class="finance-card flex flex-col text-center p-2.5">
    <h3 class="font-medium">{{ label }}</h3>
    <p class="font-medium text-2xl" :style="{ color: valueColor }">
      {{ formattedValue }}
    </p>
  </div>
</template>

<script lang="ts">
import { computed, defineComponent } from 'vue'

export default defineComponent({
  name: 'FinanceCardItem',
  props: {
    label: {
      type: String,
      required: true,
    },
    value: {
      type: Number,
      required: true,
    },
    valueColor: {
      type: String,
      required: true,
    },
  },
  setup(props) {
    const formattedValue = computed(() => `${(props.value ?? 0).toFixed(2)} €`)

    return {
      formattedValue,
    }
  },
})
</script>
