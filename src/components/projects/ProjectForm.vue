<script setup>
import { ref } from 'vue'

const props = defineProps({
    project: {
        type: Object,
        required: true
    },
    title: {
        type: String,
        default: 'Project'
    }
})

const emit = defineEmits(['save', 'cancel'])

const thisRefIsForTestingOnly_ShowErrorMessages = ref(true)

const getErrorOfField = (fieldName) => {
    return `Error message for ${fieldName} field` 
}

const clickSave = (project) => {
    emit('save', project)
}

const clickCancel = () => {
    emit('cancel')
}

</script>

<template>
    <div class="py-4 px-2">
        <div class="flex flex-col space-y-3 px-3">
            <!-- Next DIV is just for testing purposes -->
            <div class="flex space-x-1 align-middle">
                <label class="font-medium text-sm leading-8">Show Error Messages (for testing purposes only)</label>
                <input type="checkbox"  class="mt-[0.40rem] w-5 h-5 border-gray-300 border"
                        v-model="thisRefIsForTestingOnly_ShowErrorMessages">     
            </div>
            <h2 class="text-2xl">
                {{ title }}
            </h2>

            <div class="flex flex-col">
                <div class="flex space-x-1 align-middle">
                    <label for="input_name_id" class="w-24 font-medium text-sm leading-10">Name</label>
                    <input type="text" id="input_name_id" class="px-4 grow h-10 border-gray-300 border rounded-lg text-base"
                            v-model="project.name">     
                </div>                
                <ErrorMessage class="ps-[6.5rem]" :errorMessage="getErrorOfField('name')" v-show="thisRefIsForTestingOnly_ShowErrorMessages"></ErrorMessage>
            </div>
            <div class="pt-4 flex space-x-4 justify-end">
                <button type="button" class="w-24 h-10 text-sm font-bold rounded-md 
                                            border border-transparent bg-gray-400 text-white 
                                            hover:bg-gray-500 focus:outline-none focus:bg-gray-500"
                        @click.prevent="clickCancel">
                    Cancel
                </button>                
                <button type="button" class="w-24 h-10 text-sm font-bold rounded-md 
                                            border border-transparent bg-green-700 text-white 
                                            hover:bg-green-800 focus:outline-none focus:bg-green-800"
                        @click.prevent="clickSave(project)">
                    Save
                </button>  
            </div>
        </div>
    </div>
</template>