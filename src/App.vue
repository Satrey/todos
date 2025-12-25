<template>
  <div id="app">
    <current-time class="col-2"/>
    <task-input class="col-6" @add-task="addNewTask"/>
    <div class="cardBox col-6">
      <div class="sorter">
          <div>
            <input
              v-model="hideDone"
              type="checkbox"
              id="hideDone"
              name="hideDone"
            />
            <label for="hideDone">
              Hide Done Tasks
            </label>
          </div>
          <div>
            <input
              v-model="reverse"
              type="checkbox"
              id="reverse"
              name="reverse"
            />
            <label for="reverse">
              Reverse Order
            </label>
          </div>
          <div>
            <input
              v-model="sortById"
              type="checkbox"
              id="sortById"
              name="sortById"
            />
              <label for="sortById">
              Sort By Id
            </label>
          </div>
        </div>
    </div>
    <div class='col-12'>
      <div class='cardBox col-6'>        
        <table class="tasksTable">
          <thead>
            <th>Номер</th>
            <th>Открыто</th>
            <th>Наименование</th>
            <th></th>
            <th>Закрыта</th>
          </thead>
          <tbody>
              <tr v-for='(taskItem, index) in displayList'
              :key='`${index}_${Math.random()}`'
              >
                <td>{{ taskItem.id }}</td>
                <td>{{ formateDate(taskItem.createdAt) }}</td>
                <td>{{ taskItem.task }}</td>
                <td>
                  <input type='checkbox'
                  :checked='!!taskItem.finishedAt'
                  @input='changeStatus(taskItem.id)'
                  />
                </td>
                <td>
                  <span v-if='taskItem.finishedAt'> 
                    {{ formateDate(taskItem.finishedAt) }}
                  </span>
                </td>
              </tr>
          </tbody>
        </table>
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
    hideDone: false,
    reverse: false,
    sortById: false,
  }),

  computed: {
    baseList() {
      return [...this.taskList].map(
        (t, index) => ({
          ...t, id: index +1
        })
      );
    },

    filteredList() {
      return [...this.baseList].filter(t => !t.finishedAt)
    },

    sortedList() {
      return [...this.filteredList].sort((a,b) => b.id - a.id);
    },

    displayList() {
      return this.sortedList;
    },
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

    changeStatus(taskId){
      const task = this.taskList[taskId - 1];
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