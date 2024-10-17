<template>
    <div class="relative">
        <input
            type="text"
            class="form-control form-control-lg padding-right-lg bg-success-light border-2"
            placeholder="Got a new task? Type it in and click Add task."
            v-model="name"
        />
        <input
            type="text"
            class="form-control form-control-sm padding-right-lg bg-success-light border-2 mt-1 fst-italic"
            placeholder="Please enter a short description."
            v-model="description"
        />
        <div class="text-danger" v-if="error">
            <p>⚠ Please enter a valid task and description</p>
        </div>
        <div class="text-end my-3">
            <button
                class="btn btn-sm btn-success btn-block"
                @click="addNewTask()"
            >
                + Add task
            </button>
        </div>
    </div>
</template>

<script setup>
import { ref, reactive } from 'vue';
import { useTaskStore } from '../../stores/task';

const error = ref(false);
const name = ref('');
const description = ref('');

const store = useTaskStore();
const { handleAddedTask } = store;

const newTask = reactive({
    name: '',
    description: '',
    is_completed: false,
});

// const addNewTask = (event) => {
//     if (event.target.value.trim()) {
//         newTask.name = event.target.value;
//         event.target.value = '';
//         emit('added', newTask);
//     }
// };
const addNewTask = async () => {
    if (name.value.trim() && description.value.trim()) {
        error.value = false;
        newTask.name = name.value.trim();
        newTask.description = description.value.trim();
        console.log(newTask);
        name.value = '';
        description.value = '';
        await handleAddedTask(newTask);
    } else {
        error.value = true;
        console.error('Empty!');
    }
};
</script>
