<script setup>
import { ref, computed, provide } from 'vue'
import ProjectList from './ProjectList.vue'

const projects = ref([
    {
        'id': 1,
        'name': 'Project Test 1',
        'created_by_id' : 1,
        'created_by_name' : 'John Doe'
    },
    {
        'id': 2,
        'name': 'Project Test 2',
        'created_by_id' : 2,
        'created_by_name' : 'Mary Jane'
    },
    {
        'id': 3,
        'name': 'Unknown Project for App Test',
        'created_by_id' : 1,
        'created_by_name' : 'John Doe'
    },
])

provide('projects', projects)

const totalProjects = computed(() => {
  return projects.value ? projects.value.length : 0
})

const insertProject = () => {
    console.log('insert project')
}
</script>

<template>
    <div class="pt-4">
        <button type="button" class="mt-4 w-36 h-10 text-sm font-bold rounded-md 
                                    border border-transparent bg-blue-600 text-white 
                                    hover:bg-blue-700 focus:outline-none focus:bg-blue-700"
                @click.prevent="insertProject">
            New Project
        </button>        
        <h2 class="pt-8 pb-2 text-2xl">
            Projects
            <span class="text-base">(Total = {{ totalProjects }})</span> 
        </h2>
        <div v-show="totalProjects > 0">
            <ProjectList :projects="projects"></ProjectList>
        </div>
    </div>
</template>