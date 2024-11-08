<script setup>
import axios from 'axios'
import { ref, onMounted, useTemplateRef, inject } from 'vue'

const tasks = inject('tasks')
const newTask = ref('')
const newProject = ref(null)
const listOfProjects = ref([])
const errorMessage = ref('')

// "taskInput" object references the input HTML element with 
// the ref attribute set to "taskinput"
const taskInput = useTemplateRef('taskinput')

const addTask = async () => {
    errorMessage.value = ''
    try {
        if (newTask.value.trim()) {
            let taskToInsert = {
                description: newTask.value,
                completed: false,
                project_id: newProject.value?.id ?? null
            }
            const response = await axios.post('tasks', taskToInsert)
            let insertedTask = response.data.data
            tasks.value.push(insertedTask)        
        }
        newTask.value = ''
    } catch (error) {
        errorMessage.value = error.response.data.message
    } finally {
        taskInput.value.focus()
    }    
}

const loadProjects = async () => {
    const response = await axios.get('projects')
    listOfProjects.value = response.data.data
    listOfProjects.value.unshift(null)
}

onMounted(() => {
    loadProjects()
    taskInput.value.focus()
})
</script>

<template>
    <div class="py-2">
        <label for="input_new_task_id" class="font-medium">New Task</label>
        <div class="flex rounded-lg flex-wrap">
            <input ref="taskinput" type="text" id="input_new_task_id"
                class="p-2 grow h-10 border-gray-300 border rounded-s-lg text-base"
                v-model="newTask"
                v-on:keydown.enter="addTask">                
            <select class="py-2 px-4 h-10 border-gray-300 border text-base" 
                v-model="newProject">
                <option v-for="p in listOfProjects" :value="p">{{ p ? p.name: '-- No Project --' }}</option>
            </select>
            <button type="button" class="w-10 h-10 shrink-0 inline-flex justify-center items-center gap-x-2 text-sm font-bold rounded-e-md 
                                        border border-transparent bg-blue-600 text-white 
                                        hover:bg-blue-700 focus:outline-none focus:bg-blue-700"
                    @click.prevent="addTask">
                <svg class="shrink-0 size-6" xmlns="http://www.w3.org/2000/svg" 
                    fill="none" viewBox="0 0 24 24" stroke-width="3" stroke="currentColor">
                    <path stroke-linecap="round" stroke-linejoin="round" d="M12 4.5v15m7.5-7.5h-15" />
                </svg>
            </button>
        </div>
        <ErrorMessage :errorMessage="errorMessage"></ErrorMessage>
    </div>
</template>