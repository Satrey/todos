<template>
  <div id="app">
    <current-time class="col-2"/>
    <task-input class="col-6" @add-task="addNewTask"/>
    <div class='col-12'>
      <div class='cardBox col-6'>
        <div class='container'>
          <h2>My Tasks</h2>

          <table class="tasksTable">
            <thead>
              <th>Номер</th>
              <th>Открыто</th>
              <th>Наименование</th>
              <th>Закрыта</th>
            </thead>
            <tbody>
                <tr v-for='(taskItem, index) in displayList'
                :key='`${index}_${Math.random()}`'
                >
                  <td>{{ index + 1 }}</td>
                  <td>{{ formateDate(taskItem.createdAt) }}</td>
                  <td>{{ taskItem.task }}</td>
                  <td>
                    <input type='checkbox'
                    :checked='!!taskItem.finishedAt'
                    @input='changeStatus(index)'
                    />
                    <span v-if='taskItem.finishedAt'> 
                      {{ formateDate(taskItem.finishedAt) }}
                    </span>
                  </td>
                </tr>
            </tbody>
          </table>
          <!-- <ol class='taskList'>
            <li
            v-for='(taskItem, index) in displayList'
            :key='`${index}_${Math.random()}`'
            >
              <hr>
              {{ formateDate(taskItem.createdAt) }}
              {{ taskItem.task }}
              <input type='checkbox'
              :checked='!!taskItem.finishedAt'
              @input='changeStatus(index)'
              />
              <span v-if='taskItem.finishedAt'>
                Выполнено : 
                {{ formateDate(taskItem.finishedAt) }}
              </span>
              <hr>
            </li>
          </ol> -->
        </div>
      </div>
    </div>
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

  data: () => ({
    taskList: [],
  }),

  computed: {
    displayList() {
      return this.taskList;
    }
  },

  methods: {
    formateDate(value) {
      if (!value) return '';
      if (typeof value !== 'number') return value;

      const browserLocale = 
        navigator.languages && navigator.languages.length
          ? navigator.languages[0]
          : navigator.language;
      const intlDateTime = new Intl.DateTimeFormat(
        browserLocale,
        {
          year: 'numeric',
          month: 'numeric',
          day: 'numeric',
          hour: 'numeric',
          minute: 'numeric'
        });
      return intlDateTime.format(new Date(value));
    },

    addNewTask(task){
      this.taskList.push({
        task,
        createdAt: Date.now(),
        finishedAt: undefined,
      })
    },

    changeStatus(taskIndex){
      const task = this.taskList[taskIndex];
        if(task.finishedAt){
          task.finishedAt = undefined;
        } else {
          task.finishedAt = Date.now();
        }
    },
  },
}
</script>


<style scoped>
.taskList li{
  text-align: left;
}
</style>