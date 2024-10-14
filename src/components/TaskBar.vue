<template>
  <div
    class="flex justify-evenly border-0 border-t-2 border-t-zinc-200 items-center p-2 m-2"
  >
    <Icon icon="ic:round-home" class="h-6 w-6" />
    <Icon icon="mdi:calendar-outline" class="h-6 w-6" />
    <Icon icon="streamline:add-circle-solid" class="h-8 w-8 text-blue-400" />
    <Icon icon="mdi:chart-line" class="h-6 w-6" />
    <Icon icon="ri:more-fill" class="h-6 w-6" />
  </div>
</template>

<script lang="ts">
import { Icon } from '@iconify/vue'
import { defineComponent } from 'vue'

export default defineComponent({
  name: 'TaskBar',
  components: {
    Icon,
  },
})
</script>
