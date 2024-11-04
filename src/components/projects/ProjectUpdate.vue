<script setup>
import { ref, watch } from 'vue'
import ProjectForm from './ProjectForm.vue'

const project = ref(null)

const props = defineProps({
    id: {
        type: Number,
        required: true
    }
})

// When the "id" prop changes, the "task" also changes
// Later, we will load the project from the API using the ID
watch(
    () => props.id,
    (newValue) => {
        project.value = {
            id: newValue,
            name: 'Project ' + newValue
        }
    },
    { immediate: true }
)

const save = (project) => {
    console.log('save updated project')
    console.log(project)
}

const cancel = () => {
    console.log('cancel project update')
}
</script>

<template>
    <ProjectForm :project="project" :title="`Update project # ${project.id}`" @save="save" @cancel="cancel"></ProjectForm>
</template>