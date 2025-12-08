<script setup>
const props = defineProps({
  title: {
    type: String,
    required: true
  },
  description: {
    type: String,
    default: ""
  }
})
</script>
<template>
  <article class="rounded-lg bg-gray-800 p-4 shadow-md border border-gray700">
    <h3 class="font-semibold text-sm text-white">
      {{ props.title }}
    </h3>
    <p v-if="props.description" class="text-xs text-gray-300 mt-1 leadingrelaxed">
      {{ props.description }}
    </p>
  </article>
</template>
