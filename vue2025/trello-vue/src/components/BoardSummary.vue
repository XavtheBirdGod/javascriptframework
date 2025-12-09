<script setup>
const props = defineProps({
  columns: {
    type: Array,
    required: true
  }
})
</script>

<template>
  <div class="p-4 mb-6 bg-gray-800 rounded-lg shadow-md">
    <h2 class="text-xl font-semibold mb-3 text-purple-400">
      Overzicht van de kolommen
    </h2>

    <ul class="space-y-2">
      <li
          v-for="column in columns"
          :key="column.id"
          class="flex justify-between border-b border-gray-700 pb-1"
      >
        <span>{{ column.title }}</span>
        <span>{{ column.tasks.length }} taken</span>
      </li>
    </ul>
  </div>
</template>
