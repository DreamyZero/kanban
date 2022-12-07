<template>
    <div @dragstart="dragStart" draggable="true" class="task">
        <div class="task__header">
            <span class="task__title">Задача №{{ task.id }}</span>
            <div
                class="task__priority"
                :class="'task__priority--' + task.priority"
            >
                {{ task.priority }}
            </div>
        </div>
        <p class="task__description">{{ task.description }}</p>
        <p class="task__date">{{ date }}</p>
        <div class="task__functions">
            <button
                :disabled="task.status == 'todo'"
                @click="$emit('move', task.id, 'left')"
                class="task__button"
            >
                <svg version="1.1" xmlns="http://www.w3.org/2000/svg" width="25px" height="25px" viewBox="0 0 96 96" enable-background="new 0 0 96 96" xml:space="preserve">
                    <path fill="currentColor" d="M83.999,43.999H21.656l21.172-21.172c1.562-1.562,1.561-4.094,0-5.657c-1.562-1.562-4.094-1.562-5.657,0l-28,28
                            c-1.562,1.562-1.562,4.095-0.001,5.656l28,28c0.781,0.781,1.805,1.172,2.829,1.172c1.023,0,2.047-0.391,2.829-1.173
                            c1.562-1.561,1.561-4.094,0-5.655L21.655,51.999h62.344c2.209,0,4.001-1.792,4-4C87.999,45.79,86.208,43.999,83.999,43.999z"/>
                </svg>
            </button>
            <button
                @click="$emit('editTask')"
                class="task__button"
            >
                Edit
            </button>
            <button
                v-if="task.status=='completed'"
                @click="$emit('move', task.id, 'right')"
                class="task__button"
            >
                x
            </button>
            <button
                v-else
                @click="$emit('move', task.id, 'right')"
                class="task__button"
            >
                <svg version="1.1" xmlns="http://www.w3.org/2000/svg" style="transform: rotate(180deg)" width="25px" height="25px" viewBox="0 0 96 96" enable-background="new 0 0 96 96" xml:space="preserve">
                    <path fill="currentColor" d="M83.999,43.999H21.656l21.172-21.172c1.562-1.562,1.561-4.094,0-5.657c-1.562-1.562-4.094-1.562-5.657,0l-28,28
                            c-1.562,1.562-1.562,4.095-0.001,5.656l28,28c0.781,0.781,1.805,1.172,2.829,1.172c1.023,0,2.047-0.391,2.829-1.173
                            c1.562-1.561,1.561-4.094,0-5.655L21.655,51.999h62.344c2.209,0,4.001-1.792,4-4C87.999,45.79,86.208,43.999,83.999,43.999z"/>
                </svg>
            </button>
            
        </div>
    </div>
</template>

<script>
export default {
    name: 'VTask',
    props: {
        task: {
            id: Number,
            description: String,
            createdAt: Date,
            status: String,
            priority: Number
        }
    },
    computed: {
        date() {
            return this.task.createdAt.toLocaleDateString('ru');
        }
    },
    methods: {
        dragStart(event) {
            event.dataTransfer.setData('task', this.task.id)
        }
    }
}
</script>