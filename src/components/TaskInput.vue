<template>
    <div class="cardBox">
        <div class="container">
            <strong>Моя задача :</strong>
            <input type="text" v-model="task" class="taskInput">
            <button v-on:click="addTask">Добавить задачу</button>
        </div>
    </div>
</template>

<script>
    export default {
        name: 'TaskInput',

        data: () => ({
            task: ''
        }),

        methods: {
            addTask() {
                this.$emit('add-task', this.task);
                this.task = '';
            },
        }
    };
</script>