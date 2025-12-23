<template>
  <div id="app">
    <current-time class="col-2"/>
    <task-input class="col-6" @add-task="addNewTask"/>
  </div>
</template>

<script>
import CurrentTime from './components/CurrentTime.vue';
import TaskInput from './components/TaskInput.vue';

export default {
  name: 'app',
  components: {
    CurrentTime,
    TaskInput,
  },

  methods: {
    addNewTask(task){
      alert(`Новая задача добавлена ${task}`)
    }
  },
}
</script>
