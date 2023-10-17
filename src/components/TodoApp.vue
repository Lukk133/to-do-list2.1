<template>
  <div class="container mt-5">
    <h2 class="text-center">To do list</h2>
    <div class="d-flex">
      <input v-model="task" type="text" class="form-control" placeholder="Wprowadź zadanie" :maxlength="maxLength">
      <button class="btn btn-primary rounded-0 " @click="submitTask" :disabled="!task.length" :hidden="schowajEdytuj">
        EDYTUJ</button>
      <button :hidden="schowajDodaj" class="btn btn-primary rounded-0 " @click="submitTask" :disabled="!task.length">
        DODAJ</button>
      <button class="btn btn-primary rounded-0 " @click="cofnijEdytowanie(isClickedCofnijEdytowanie)"
        :disabled="!isClickedEdit">
        COFNIJ EDYTOWANIE</button>
    </div>

    <table class="table table-bordered mt-2">
      <thead>
        <tr>
          <th scope="col" class="text-center">Task</th>
          <th scope="col" class="text-center status" style="width: 200px">Status</th>
          <th scope="col" style="width: 200px" class="text-center">Edytuj</th>
          <th scope="col" style="width: 200px" class="text-center">Usuń</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(task, index) in tasks" :key="index" :hidden="task.status === 'W trakcie' && isClickedTodos === false || task.status === 'Zrobione' && isClickedTodos === false ||
          task.status === 'Do zrobienia' && isClickedInProgress === false || task.status === 'Zrobione' && isClickedInProgress === false ||
          task.status === 'Do zrobienia' && isClickedFinished === false || task.status === 'W trakcie' && isClickedFinished === false
          ">
          <td>

            <span :class="{ 'finished': task.status === 'Zrobione' }">
              {{ task.name }}
            </span>


            <!--:class="{ 'todoMark': isClickedTodos === false && task.status === 'Do zrobienia' }-->

          </td>
          <td class="text-center" style="width: 120px"><span style="cursor: pointer" @click="changeStatus(index)" :class="{
            'text-danger': task.status === 'Do zrobienia',
            'text-warning': task.status === 'W trakcie',
            'text-success': task.status === 'Zrobione'
          }">
              {{ task.status }}
            </span>
          </td>
          <td>
            <div @click="enableEditReturn(isClickedEdit)">
              <div class="text-center" @click="editTask(index)">
                <span class="fa fa-pen" style="cursor: pointer ;"></span>
              </div>
            </div>

          </td>
          <td>
            <div class="text-center" @click="deleteTask(index)">
              <span style="cursor: pointer" class="fa fa-trash"></span>
            </div>
          </td>
        </tr>




      </tbody>
    </table>
    <div class="flexbox">
      <button  class="btn btn-primary rounded-0 flex1" @click="selectTodos(isClickedTodos)"
        :class="{ 'clickedButton': isClickedTodos === false }">Wybierz wszystkie do zrobienia</button>
      <button  class="btn btn-secondary rounded-0 flex1" @click="selectInProgress(isClickedInProgress)"
        :class="{ 'clickedButton2': isClickedInProgress === false }">Wybierz wszystkie w trakcie</button>
      <button class="btn btn-primary rounded-0 flex1" @click="selectFinished(isClickedFinished)"
        :class="{ 'clickedButton': isClickedFinished === false }">Wybierz wszystkie zrobione</button>
    </div>

  </div>

  <!-- użyć :hidden na cały row czyli tr
    script setup nie działa <div :value="value" hidden>wadwaijd;l
     <input v-model="text" type="input" />
  <div>{{ this.text }}</div>
  </div>-->
</template>

<script>

import { ref, watch } from 'vue'

const text = ref("Unknown");

const STORAGE_KEY = 'vue-todo-app-storage';

localStorage.setItem('taskName', 'HALO');

//if(task.length === 50){alert('halo')}
//if (this.task.length === 50){alert('Maksymalna liczba znaków to 50')} 


export default {
  name: 'HelloWorld',
  props: {
    msg: String
  },
  data() {
    return {
      schowajEdytuj: true,
      schowajDodaj: false,
      ukryjDoZrobienia: true,
      maxLength: 50,
      isClickedTodos: true,
      isClickedInProgress: true,
      isClickedFinished: true,
      isClickedEdit: false,
      isClickedCofnijEdytowanie: false,
      task: '',
      editedTask: null,
      availableStatuses: ['Do zrobienia', 'W trakcie', 'Zrobione'],
      tasks: [
        {
          name: 'Zrobić pranie',
          status: 'Do zrobienia'
        },
        {
          name: 'Ugotować obiad',
          status: 'Do zrobienia'
        }
      ]
    }
  },
  methods: {
    submitTask() {
      if (this.task.length === 0) return;

      if (this.task.length === 50) { alert('Maksymalna liczba znaków to 50') };//nwm jak inaczej

      if (this.editedTask === null) {
        this.tasks.push({
          name: this.task,
          status: 'Do zrobienia',
        })
      } else {
        this.tasks[this.editedTask].name = this.task
        this.editedTask = null
        this.schowajDodaj = false
        this.schowajEdytuj = true;
      }
      this.task = ''
      this.isClickedEdit = false

      localStorage.setItem(STORAGE_KEY, JSON.stringify(this.tasks))
    },
    created() {
      this.todos = JSON.parse(localStorage.getItem(STORAGE_KEY) || '[]')
    },
    deleteTask(index) {
      this.tasks.splice(index, 1)
      localStorage.setItem(STORAGE_KEY, JSON.stringify(this.tasks))
    },
    editTask(index) {
      this.task = this.tasks[index].name
      this.editedTask = index
      localStorage.setItem(STORAGE_KEY, JSON.stringify(this.tasks))
      this.schowajDodaj = true
      this.schowajEdytuj = false

      // this.dodaj = edytuj
      // if true dodaj = edytuj
    },
    changeStatus(index) {
      let newIndex = this.availableStatuses.indexOf(this.tasks[index].status)
      if (++newIndex > 2) newIndex = 0;
      this.tasks[index].status = this.availableStatuses[newIndex]
      localStorage.setItem(STORAGE_KEY, (this.tasks.status))
    },
    selectTodos(value) {
      this.isClickedTodos = !value
    },
    selectInProgress(value) {
      this.isClickedInProgress = !value
    },
    selectFinished(value) {
      this.isClickedFinished = !value
    },
    enableEditReturn(value) {
      this.isClickedEdit = !value
    },
    cofnijEdytowanie(value) {
      this.isClickedCofnijEdytowanie = !value
      if (this.isClickedCofnijEdytowanie = true) {
        this.task = ''
        this.isClickedEdit = false
      }
    }
  }
}
//disabled!!

localStorage.getItem('taskName')

</script>
<style scoped>
.finished {
  text-decoration: line-through;
}
.flexbox{
        display: flex;
        flex-direction: row;
    }
.flex1{
    flex: 1;
}
/*.flex{
  display: grid;
  
}
.flex1 .flex2 .flex3{
  grid-template-columns: 1fr 1fr 1fr;
}*/
.clickedButton {
  background-color: #0a4aa8
}

.clickedButton2 {
  background-color: rgb(66, 69, 71);
}
</style>


