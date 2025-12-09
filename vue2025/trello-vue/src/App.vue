<script setup>
import { ref } from "vue"
import TaskCard from "./components/TaskCard.vue"
import BoardColumn from "./components/BoardColumn.vue"
import BoardSummary from "./components/BoardSummary.vue"

const newTaskTitle = ref("")
const newTaskDescription = ref("")
const newTaskColumnId = ref("original")

const columns = ref([{
  id: "original",
  title: "Taken (origineel)",
  color: "blue",
  tasks: [
    {
      id: 1,
      title: "Nieuwe cursusstructuur uitwerken",
      description: "Modules herzien en oefeningen toevoegen.",
      assignee: "Tom Vanhoutte",
      role: "Full stack developer",
      priority: "hoog",
      done: false,
    },
    {
      id: 2,
      title: "Design voor dashboard maken",
      description: "Wireframes en kleurenpalet bepalen.",
      assignee: "Sarah Janssens",
      role: "UI designer",
      priority: "middel",
      done: false,
    },
    {
      id: 3,
      title: "Component structuur bepaald",
      description: "",
      assignee: "Noah Martens",
      role: "Software architect",
      priority: "middel",
      done: true,
    },
  ],
},

  {
    id: "byRole",
    title: "Taken (op functie)",
    color: "green",

    tasks: [
      {
        id: 4,
        title: "Nieuwe cursusstructuur uitwerken (R)",
        description: "Modules herzien en oefeningen toevoegen.",
        assignee: "Tom Vanhoutte",
        role: "Full stack developer",
        priority: "hoog",
        done: false,
      },
      {
        id: 5,
        title: "Component structuur bepaald (R)",
        description: "",
        assignee: "Noah Martens",
        role: "Software architect",
        priority: "middel",
        done: true,
      },
      {
        id: 6,
        title: "Design voor dashboard maken (R)",
        description: "Wireframes en kleurenpalet bepalen.",
        assignee: "Sarah Janssens",
        role: "UI designer",
        priority: "middel",
        done: false,
      },
    ],
  },
  {
    id: "backlog",
    title: "Backlog",
    color: "purple",

    tasks: [
      {
        id: 7,
        title: "En ik moet nu een naam verzinnen ofwa",
        description: "meow meow meow meow meow meow meow meow",
        assignee: "De Rizzler",
        role: "Freaklord",
        priority: "hoog",
        done: false,
      },
    ],
  }
]);

const nextTaskId = ref(100);
const handleAddTask = () => {
  const newTask = {
    id: nextTaskId.value++,
    title: newTaskTitle.value,
    description: newTaskDescription.value,
    assignee: "N.N.",
    role: "New Task",
    priority: "laag",
    done: false,
  };

  const targetColumn = columns.value.find(
      (col) => col.id === newTaskColumnId.value
  );

  if (targetColumn) {
    targetColumn.tasks.push(newTask);
  }

  newTaskTitle.value = "";
  newTaskDescription.value = "";
  newTaskColumnId.value = "original";
};

function handleAddTaskToColumn(payload) {
  const column = columns.value.find(c => c.id === payload.columnId)

  if (!column) return

  column.tasks.push({
    id: nextTaskId.value++,
    title: payload.title,
    description: "",
    assignee: "Onbekend",
    role: "Onbekend",
    priority: "",
    done: false
  })
}
</script>

<template>
  <main class="min-h-screen bg-gray-900 text-white px-6 py-8">
    <header class="mb-6">
      <h1 class="text-2xl font-bold">
        Mijn kanban board (dynamische versie)
      </h1>
      <p class="text-gray-400 text-sm mt-1">
        Nieuwe taken toevoegen aan de kolommen.
      </p>
    </header>

    <BoardSummary :columns="columns" />


    <form @submit.prevent="handleAddTask" class="mb-8 p-4 bg-gray-800 rounded-lg shadow-md">
      <h2 class="text-xl font-semibold mb-4 text-blue-400">Nieuwe taak toevoegen</h2>

      <div class="grid grid-cols-1 md:grid-cols-3 gap-4 mb-4">
        <div class="col-span-1">
          <label for="title" class="block text-sm font-medium text-gray-300">Titel</label>
          <input
              id="title"
              type="text"
              v-model="newTaskTitle"
              placeholder="Korte beschrijving van de taak"
              class="mt-1 block w-full rounded-md border-gray-600 bg-gray-700 text-white p-2 focus:border-blue-500 focus:ring-blue-500"
          >
        </div>

        <div class="col-span-1">
          <label for="column" class="block text-sm font-medium text-gray-300">Kies kolom</label>
          <select
              id="column"
              v-model="newTaskColumnId"
              class="mt-1 block w-full rounded-md border-gray-600 bg-gray-700 text-white p-2 focus:border-blue-500 focus:ring-blue-500"
          >
            <option
                v-for="column in columns"
                :key="column.id"
                :value="column.id"
            >
              {{ column.title }}
            </option>
          </select>
        </div>
      </div>

      <div class="mb-4">
        <label for="description" class="block text-sm font-medium text-gray-300">Beschrijving</label>
        <textarea
            id="description"
            v-model="newTaskDescription"
            rows="2"
            placeholder="Uitgebreide beschrijving van de vereisten..."
            class="mt-1 block w-full rounded-md border-gray-600 bg-gray-700 text-white p-2 focus:border-blue-500 focus:ring-blue-500"
        ></textarea>
      </div>

      <button
          type="submit"
          class="px-4 py-2 bg-blue-600 text-white font-semibold rounded-md hover:bg-blue-700 transition duration-150 ease-in-out disabled:bg-gray-500 disabled:cursor-not-allowed"
          :disabled="newTaskTitle.trim().length === 0"
      >
        Taak toevoegen
      </button>
    </form>


    <section class="flex gap-6 overflow-x-auto pb-4">
      <BoardColumn
          v-for="column in columns"
          :key="column.id"
          :id="column.id"
          :title="column.title"
          :count="column.tasks.length"
          :color="column.color"
          @add-task="handleAddTaskToColumn"
      >
        <TaskCard
            v-for="task in column.tasks"
            :key="task.id"
            :title="task.title"
            :description="task.description"
            :assignee="task.assignee"
            :role="task.role"
            :priority="task.priority"
            :done="task.done"
        />
      </BoardColumn>
    </section>
  </main>
</template>