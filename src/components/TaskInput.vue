<template>
    <div class="cardBox col-6">
        <div class="sorter">
            <div style="align-content: center;">Моя задача :</div>
            <input type="text" v-model="task" class="taskInput">
            <button v-on:click="addTask" class="addButton">Добавить задачу</button>
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