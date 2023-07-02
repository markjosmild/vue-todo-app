<script setup>
import { ref, onMounted, computed, watch } from 'vue'
const todos = ref([])
const name = ref('')
const input_content = ref('')
watch(name, (newVal) => {
  localStorage.setItem('name', newVal)
})
watch(todos, (newVal) => {
  localStorage.setItem('todos', JSON.stringify(newVal))
}, { deep: true })
onMounted(() => {
  name.value = localStorage.getItem('name') || ''
  todos.value = JSON.parse(localStorage.getItem('todos')) || []
})
const addTodo = () => {
  if (input_content.value === "") {
    return
  }
  todos.value.push({
    id: crypto.randomUUID(),
    taskName: input_content.value,
    completed: false,
  })
  input_content.value = ""
}
const deleteTodo = (id) => {
  todos.value = todos.value.filter(todo => todo.id !== id);
}
</script>
<template>
  <div class="h-screen">
    <div class="flex flex-col justify-center items-center gap-2">
      <h2 class="text-4xl font-bold">YOUR TODO LIST USING VUE</h2>
      <h2 class="flex justify-center font-semibold">
        <input class="text-center appearance-none bg-transparent border-none focus:outline-none focus:border-blue-500"
          type="text" placeholder="Enter your name here..." v-model="name" />
      </h2>
      <form class="flex flex-col gap-2 items-center content-center" @submit.prevent="addTodo">
        <h3 class="text-xl">What are your todos?</h3>
        <div class="flex gap-1">
          <input class="text-center appearance-none bg-slate-600  focus:outline-none focus:border-white" type="text"
            placeholder="Enter a todo here..." v-model="input_content" />
          <input class="bg-blue-500 hover:cursor-pointer hover:bg-blue-700 text-white font-bold py-2 px-4 rounded"
            type="submit" value="Add Todo" />
        </div>
      </form>
      <div>
        <h2 v-if="todos.length === 0" class="text-lg font-bold text-red-600">No todos found</h2>
        <h2 v-else-if="todos.length === 1" class="text-lg font-bold text-green-500">Here is your todo list</h2>
        <h2 v-else class="text-lg font-bold text-green-600">Here are your todo list</h2>
        <ul>
          <li v-for="todo in todos" :class="['', { 'text-green-500': todo.completed, 'text-red-500': !todo.completed }]"
            class="flex items-center mt-2">
            <input type="checkbox" class="form-checkbox text-blue-500 h-5 w-5" v-model="todo.completed" />
            <span class="ml-2">{{ todo.taskName }}</span>
            <button @click="() => deleteTodo(todo.id)"
              class="flex-1 ml-2 px-2 py-1 bg-red-500 hover:bg-red-700 text-white font-bold rounded">Delete</button>
          </li>
        </ul>
      </div>
    </div>
  </div>
</template>

