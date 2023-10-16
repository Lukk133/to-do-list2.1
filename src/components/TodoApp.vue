<template>
  <div class="container mt-5">
    <h2 class="text-center">To do list</h2>
    <div class="d-flex">
      <input v-model="task" type="text" class="form-control" placeholder="Wprowadź zadanie" :maxlength="maxLength">
      <button id="bn" class="dodaj btn btn-warning rounded-0 "
       @click="submitTask"
       :disabled="!task.length">
       DODAJ</button>
       <button id="bn" class="dodaj btn btn-warning rounded-0 "
       @click="cofnijEdytowanie(isClickedCofnijEdytowanie)"
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
        <tr v-for="(task, index) in tasks" :key="index">
          <td>
            <div :class="{ 'todoMark': isClickedTodos === false && task.status === 'Do zrobienia' }">
              <div :class="{ 'inProgressMark': isClickedInProgress === false && task.status === 'W trakcie' }">
                <div :class="{ 'finishedMark': isClickedFinished === false && task.status === 'Zrobione' }">
                  <span :class="{ 'finished': task.status === 'Zrobione' }">
                    {{ task.name }} 
                  </span>
                </div>

              </div>


            </div>

          </td>
          <td class="text-center" style="width: 120px; cursor: pointer;"><span @click="changeStatus(index)" :class="{
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
              <span  @click="myFunctionClick()" class="fa fa-pen" style="cursor: pointer ;"></span>
            </div>
            </div>
            
          </td>
          <td>
            <div class="text-center" style="cursor: pointer" @click="deleteTask(index)">
              <span class="fa fa-trash"></span>
            </div>
          </td>
        </tr>


      </tbody>
    </table>
    <div>
      <button style="width: 430px" class="btn btn-primary rounded-0" @click="changeBgColorTodos(isClickedTodos)"
      :class="{'clickedButton': isClickedTodos === false}"
        >Wybierz wszystkie do zrobienia</button>
      <button style="width: 430px" class="btn btn-secondary rounded-0" @click="changeBgColorInProgress(isClickedInProgress)"
      :class="{'clickedButton2': isClickedInProgress === false}"
        >Wybierz wszystkie w trakcie</button>
      <button style="width: 430px" class="btn btn-primary rounded-0" @click="changeBgColorFinished(isClickedFinished)"
      :class="{'clickedButton': isClickedFinished === false}"
        >Wybierz wszystkie skończone</button>
    </div>

  </div>
</template>

<script>

const STORAGE_KEY = 'vue-todo-app-storage';

localStorage.setItem('taskName', 'HALO');





export default {
  name: 'HelloWorld',
  props: {
    msg: String
  },
  data() {
    return {
      maxLength: 50,
      isClickedTodos: true,
      isClickedInProgress: true,
      isClickedFinished: true,
      isClickedEdit: false,
      isClickedCofnijEdytowanie: false,
      task: '',
      editedTask: null,
      el: '#app',
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

      if (this.task.length === 50){alert('Maksymalna liczba znaków to 50')} ;//nwm jak inaczej

      if (this.editedTask === null) {
        this.tasks.push({
          name: this.task,
          status: 'Do zrobienia',
        })
      } else {
        this.tasks[this.editedTask].name = this.task
        this.editedTask = null;
      }
      this.task = ''
      this.isClickedEdit = false
      
      localStorage.setItem(STORAGE_KEY, JSON.stringify(this.tasks))
    },
    created(){
      this.todos=JSON.parse(localStorage.getItem(STORAGE_KEY) || '[]')
    },
    deleteTask(index) {
      this.tasks.splice(index, 1)
      localStorage.setItem(STORAGE_KEY, JSON.stringify(this.tasks))
    },
    editTask(index) {
        this.task = this.tasks[index].name
      this.editedTask = index
      localStorage.setItem(STORAGE_KEY, JSON.stringify(this.tasks))
      
    },
    changeStatus(index) {
      let newIndex = this.availableStatuses.indexOf(this.tasks[index].status)
      

      if (++newIndex > 2) newIndex = 0;
      this.tasks[index].status = this.availableStatuses[newIndex]
    },
    changeBgColorTodos(value) {
      this.isClickedTodos = !value
    },
    changeBgColorInProgress(value) {
      this.isClickedInProgress = !value
    },
    changeBgColorFinished(value) {
      this.isClickedFinished = !value
    },
    enableEditReturn(value) {
      this.isClickedEdit = !value
    },
    cofnijEdytowanie(value) {
      this.isClickedCofnijEdytowanie = !value
      if(this.isClickedCofnijEdytowanie = true)
      {this.task = ''
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

.todoMark {
  background-color: rgb(255, 116, 116);
}

.inProgressMark {
  background-color: rgb(253, 243, 93)
}

.finishedMark {
  background-color: rgb(54, 255, 47);
}
.flex{
  display: grid;
  
}
.flex1 .flex2 .flex3{
  grid-template-columns: 1fr 1fr 1fr;
}
.clickedButton{
  background-color: #0b4fb4
}
.clickedButton2{
  background-color: #515558;
}

</style>


