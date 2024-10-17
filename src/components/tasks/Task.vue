<template>
    <li class="list-group-item py-3">
        <div class="d-flex justify-content-start align-items-center">
            <input
                class="form-check-input mt-0 completed"
                :class="completedClass"
                type="checkbox"
                :checked="task.is_completed"
                @change="markTaskAsCompleted"
            />
            <div
                class="ms-2 flex-grow-1"
                :class="completedClass"
                title="Double click the text to edit or remove"
                @dblclick="($event) => (isEdit = true)"
            >
                <div class="relative" v-if="isEdit">
                    <input
                        class="editable-task"
                        type="text"
                        v-focus
                        @keyup.esc="undo"
                        v-model="name"
                        @keyup.enter="updateTask"
                    />
                </div>
                <span v-else>{{ task.name }}</span>
                <div class="relative" v-if="isEdit">
                    <input
                        class="editable-task"
                        type="text"
                        v-focus
                        @keyup.esc="undo"
                        v-model="description"
                        @keyup.enter="updateTask"
                    />
                </div>
                <div class="description" v-else>
                    <p>{{ task.description }}</p>
                </div>
                <div class="relative" v-if="isEdit">
                    <button
                        class="btn btn-sm btn-success mt-2"
                        @click="updateTask"
                    >
                        Update
                    </button>
                </div>
            </div>
            <!-- <div class="task-date fw-bold">
                                        24 Feb 12:00
                                    </div> -->
        </div>
        <TaskActions
            @edit="($event) => (isEdit = true)"
            v-show="!isEdit"
            @remove="removeTask"
        />
    </li>
</template>

<script setup>
import { computed, ref } from 'vue';
import TaskActions from './TaskActions.vue';

const props = defineProps({
    task: Object,
});

const emit = defineEmits(['updated', 'completed', 'removed']);

const isEdit = ref(false);
// const editingTaskName = ref(props.task.name);
// const editingTaskDescription = ref(props.task.description);

const name = ref(props.task.name);
const description = ref(props.task.description);

const completedClass = computed(() =>
    props.task.is_completed ? 'completed' : ''
);

const vFocus = {
    mounted: (el) => el.focus(),
};

const updateTask = (event) => {
    const updatedTask = {
        ...props.task,
        name: name.value,
        description: description.value,
    };
    isEdit.value = false;
    emit('updated', updatedTask);
};

const undo = () => {
    isEdit.value = false;
    name.value = props.task.name;
    description.value = props.task.description;
};

const markTaskAsCompleted = (event) => {
    const updatedTask = {
        ...props.task,
        is_completed: !props.task.is_completed,
    };
    emit('completed', updatedTask);
};

const removeTask = () => {
    if (confirm('Are you sure to remove the task?')) {
        emit('removed', props.task);
    }
};
</script>
