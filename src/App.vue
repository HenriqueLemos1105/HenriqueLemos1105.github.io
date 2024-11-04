<script setup>
import axios from 'axios'
import { ref, provide, onMounted } from 'vue'
import Tasks from './components/tasks/Tasks.vue'
import { RouterView } from 'vue-router'

const tasks = ref([])
provide('tasks', tasks)

const loadTasks = async () => {
  const response = await axios.get('tasks')
  tasks.value = response.data.data
}

onMounted(() => {
  loadTasks()
})
</script>

<template>
  <div class="p-8 mx-auto max-w-3xl" id="app">
    <h1 class="text-5xl pb-8">Todolist</h1>
    <nav class="flex space-x-1 border-b-2 border-gray-800 text-base">
        <a class="w-24 h-10 leading-10 text-center rounded-t-xl border border-transparent text-white select-none
                     font-bold bg-gray-800 cursor-default">
                    Tasks
        </a>  
        <a class="w-24 h-10 leading-10 text-center rounded-t-xl border border-transparent text-white select-none
                        bg-gray-400 cursor-pointer hover:bg-gray-500 focus:outline-none focus:bg-gray-500">
                    Projects
        </a>  
      </nav>
    <Tasks></Tasks>
    <hr>
  </div>
</template>
