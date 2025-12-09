<script setup>
import { ref } from "vue"

const props = defineProps({
  id: {
    type: String,
    required: true
  },
  title: {
    type: String,
    required: true
  },
  count: {
    type: Number,
    default: 0
  },
  color: {
    type: String,
    default: "gray"
  }
})

const showForm = ref(false)
const newTitle = ref("")

const emit = defineEmits(["add-task"])

const handleColumnAddTask = () => {
  if (newTitle.value.trim().length === 0) {
    return;
  }

  emit("add-task", {
    columnId: props.id,
    title: newTitle.value
  })

  newTitle.value = ""
  showForm.value = false
}
</script>

<template>
  <section class="w-80 flex-shrink-0 bg-gray-800 rounded-xl p-4 border border-gray-700">
    <header class="flex items-center justify-between mb-3">
      <h2 class="text-sm font-semibold uppercase tracking-wide text-gray-200">
        {{ props.title }}
      </h2>

      <span
          class="inline-flex items-center justify-center rounded-full px-2 py-0.5 text-xs text-gray-100"
          :class="
          props.color === 'blue'
            ? 'bg-blue-600'
            : props.color === 'green'
              ? 'bg-green-600'
              : props.color === 'purple'
                ? 'bg-purple-600'
                : 'bg-gray-700'
        "
      >
        {{ props.count }}
      </span>
    </header>

    <div class="space-y-3">
      <slot />
    </div>

    <button
        @click="showForm = !showForm"
        class="mt-4 w-full rounded-lg py-1.5 text-xs font-medium transition-colors"
        :class="showForm ? 'bg-red-600 text-white hover:bg-red-700' : 'bg-gray-700 text-gray-200 hover:bg-gray-600'"
    >
      {{ showForm ? 'X Annuleren' : '+ kaart' }}
    </button>

    <form
        v-if="showForm"
        @submit.prevent="handleColumnAddTask"
        class="mt-3 p-3 bg-gray-700 rounded-lg"
    >
      <input
          type="text"
          v-model="newTitle"
          placeholder="Titel van de taak"
          class="w-full p-2 text-sm bg-gray-600 rounded border border-gray-500 text-white focus:ring-blue-500 focus:border-blue-500"
      >

      <button
          type="submit"
          class="mt-2 w-full py-1.5 bg-blue-600 text-white text-xs font-semibold rounded hover:bg-blue-700 transition-colors disabled:bg-gray-500 disabled:cursor-not-allowed"
          :disabled="newTitle.trim().length === 0"
      >
        Taak toevoegen
      </button>
    </form>

  </section>
</template>