<script setup>
import { ref, watch } from 'vue'
import TaskForm from './TaskForm.vue'

const task = ref(null)

const props = defineProps({
    id: {
        type: Number,
        required: true
    }
})

// When the "id" prop changes, the "task" also changes
// Later, we will load the task from the API using the ID
watch(
    () => props.id,
    (newValue) => {
        task.value = {
            id: newValue,
            description: 'Task ' + newValue,
            completed: false,
            project: null
        }
    },
    { immediate: true }
)

const save = (task) => {
    console.log('save updated task')
    console.log(task)
}

const cancel = () => {
    console.log('cancel task update')
    // Cancel
}
</script>

<template>
    <TaskForm :task="task" :title="`Update Task #${ task.id }`" @save="save" @cancel="cancel"></TaskForm>
</template>