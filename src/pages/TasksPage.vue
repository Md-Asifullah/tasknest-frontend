<template>
    <main style="min-height: 50vh; margin-top: 2rem">
        <div class="container">
            <div class="row">
                <div class="col-md-8 offset-md-2">
                    <!-- Add new Task -->
                    <NewTask />
                    <!-- List of uncompleted tasks -->
                    <tasks :tasks="uncompletedTasks" />
                    <!-- Show toggle button -->
                    <div
                        class="text-center my-3"
                        v-show="showToggleComputedBtn"
                    >
                        <button
                            class="btn btn-sm btn-success"
                            @click="
                                ($event) =>
                                    (showCompletedTasks = !showCompletedTasks)
                            "
                        >
                            <span v-if="!showCompletedTasks"
                                >Show Completed</span
                            ><span v-else>Hide Completed</span>
                        </button>
                    </div>
                    <!-- List of completed tasks -->
                    <tasks
                        :tasks="completedTasks"
                        :show="completedTasksIsVisible && showCompletedTasks"
                    />
                </div>
            </div>
        </div>
    </main>
</template>

<script setup>
import { computed, onMounted, ref } from 'vue';
import { storeToRefs } from 'pinia';
import { useTaskStore } from '../stores/task';
import Tasks from '../components/tasks/Tasks.vue';
import NewTask from '../components/tasks/NewTask.vue';
const store = useTaskStore();
const { completedTasks, uncompletedTasks } = storeToRefs(store);
const { fetchAllTasks } = store;
const showCompletedStatus = ref(false);
const showToggleComputedBtn = computed(
    () => uncompletedTasks.value.length > 0 && completedTasks.value.length > 0
);
onMounted(async () => {
    await fetchAllTasks();
});

const buttonTextDisplay = computed(() => {
    if (showCompletedStatus.value) {
        return 'Hide Completed Tasks';
    } else {
        return 'Show Completed Tasks';
    }
});

// 🚩 The below 2 statements is causing the program to break

// const uncompletedTasks = computed(() =>
//     tasks.value.filter((task) => !task.is_completed)
// );
// const completedTasks = computed(() =>
//     tasks.value.filter((task) => task.is_completed)
// );

const toggleCompleted = () => {
    showCompletedStatus.value = !showCompletedStatus.value;
};

const completedTasksIsVisible = () => {
    uncompletedTasks.value.length === 0 || completedTasks.value.length > 0;
};

const showCompletedTasks = ref(false);
</script>
