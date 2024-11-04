<script setup>
import { ref } from 'vue'
import ProjectForm from './ProjectForm.vue'

const project = ref({
    id: 0,
    name: ''
})

const create = (project) => {
    console.log('save new project')
    console.log(project)
}

const cancel = () => {
    console.log('cancel project create')
}
</script>

<template>
    <ProjectForm :project="project" title="Create new project" @save="create" @cancel="cancel"></ProjectForm>
</template>