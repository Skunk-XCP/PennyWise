<template>
  <div
    class="flex justify-evenly border-0 border-t-2 border-t-zinc-200 items-center p-2 m-2"
  >
    <button
      class="p-2 rounded-lg transition duration-200 hover:bg-gray-200 hover:ring-2 hover:ring-gray-200"
    >
      <Icon icon="ic:round-home" class="h-6 w-6" />
    </button>
    <button
      class="p-2 rounded-lg transition duration-200 hover:bg-gray-200 hover:ring-2 hover:ring-gray-200"
    >
      <Icon icon="mdi:calendar-outline" class="h-6 w-6" />
    </button>
    <button
      class="p-2 rounded-lg transition duration-200 hover:bg-gray-200 hover:ring-2 hover:ring-gray-200"
    >
      <Icon icon="streamline:add-circle-solid" class="h-8 w-8 text-blue-400" />
    </button>
    <button
      class="p-2 rounded-lg transition duration-200 hover:bg-gray-200 hover:ring-2 hover:ring-gray-200"
    >
      <Icon icon="mdi:chart-line" class="h-6 w-6" />
    </button>
    <button
      class="p-2 rounded-lg transition duration-200 hover:bg-gray-200 hover:ring-2 hover:ring-gray-200"
    >
      <Icon icon="ri:more-fill" class="h-6 w-6" />
    </button>
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
