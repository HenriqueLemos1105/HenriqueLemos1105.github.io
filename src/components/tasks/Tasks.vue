<script setup>
import { ref, computed, inject } from 'vue'
import TaskList from './TaskList.vue'
import NewTask from './NewTask.vue'
import FilterTasksForm from './TasksFilterForm.vue'

const tasks = inject('tasks')

const filterByProject = ref(null)
const filterByCompleted = ref(null)

const taskInFilter = (task) => {
    if (filterByCompleted.value) {
        if (filterByCompleted.value.completed !== task.completed) {
            return false
        }
    }
    if (filterByProject.value) {
        if (filterByProject.value.id === -1) {
            if (task.project) {
                return false
            }
        } else {
            if (!task.project) {
                return false
            }
            if (filterByProject.value.id !== task.project.id) {
                return false
            }
        }
    }
    return true
}

const filteredTasks = computed(() => tasks.value.filter(taskInFilter))

const totalTasks = computed(() => {
  return tasks.value.reduce((acumulador, task) => taskInFilter(task) ? acumulador + 1 : acumulador, 0)
})

const filterDescription = computed(() => {
    if (!filterByCompleted.value && !filterByProject.value) {
        return 'All tasks'
    }
    let description = ''
    if (filterByCompleted.value) {
        description += filterByCompleted.value.filterDescription
    } else {
        description += 'Tasks'
    }
    if (filterByProject.value) {
        if (filterByProject.value.id === -1) {
            description += ' with no project'
        } else {
            description += ' from project ' + filterByProject.value.filterDescription
        }
    }
    return description
})
</script>

<template>
    <div class="pt-4">
        <NewTask></NewTask>
        <FilterTasksForm 
            v-model:filterByProject="filterByProject" 
            v-model:filterByStatus="filterByCompleted">
        </FilterTasksForm>
        <h2 class="pt-8 pb-2 text-2xl">
            {{ filterDescription }}
            <span class="text-base">(Total = {{ totalTasks }})</span> 
        </h2>
        <div v-show="filteredTasks.length > 0">
            <TaskList :tasks="filteredTasks"></TaskList>
        </div>
    </div>
</template>