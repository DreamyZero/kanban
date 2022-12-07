<template>
    <div @dragover.prevent @drop="drop" class="board">
        <div class="board__header">
            <p class="board__title">{{ title }} ({{ tasks.length }})</p>
        </div>
        <div class="board__list">
            <VTask
                v-for="task in tasks"
                :key="task.id"
                :task="task"
                @move="moveTask"
                @edit-task="editTask(task.id)"
            />
        </div>
    </div>
</template>
<script>
import VTask from '@/components/VTask.vue';

export default {
    components: {
        VTask
    },
    props: {
        title: String,
        tasks: Array,
        code: String
    },
    methods: {
        moveTask(taskID, destination) {
            this.$emit('move', taskID, destination);
        },
        editTask(taskID) {
            this.$emit('editTask', taskID);
        },
        drop(event) {
            const taskID = event.dataTransfer.getData('task');
            this.moveTask(taskID, this.code);
        }
    }
}
</script>