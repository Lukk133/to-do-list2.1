<template>
  <div class="container mt-5">
    <h2 class="text-center">To do list</h2>
    <div class="d-flex">
      <input v-model="task" type="text" class="form-control" placeholder="Wprowadź zadanie">
      <button class="btn btn-warning rounded-0" @click="submitTask">DODAJ</button>

    </div>

    <table class="table table-bordered mt-2">
      <thead>
        <tr>
          <th scope="col" class="text-center">Task</th>
          <th scope="col" class="text-center">Status</th>
          <th scope="col" class="text-center">Edytuj</th>
          <th scope="col" class="text-center">Usuń</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(task, index) in tasks" :key="index">
          <td>
            <div :class="{'todoMark': isClickedTodos === false && task.status === 'Do zrobienia'}">
              <div :class="{'inProgressMark': isClickedInProgress === false && task.status === 'W trakcie'}">
                <div :class="{'finishedMark': isClickedFinished === false && task.status === 'Zrobione'}">
                  <span :class="{ 'finished': task.status === 'Zrobione' }">
              {{ task.name }}
            </span>
                </div>
                
              </div>
              
           
            </div>
            
          </td>
          <td style="width: 120px"><span @click="changeStatus(index)" :class="{
            'text-danger': task.status === 'Do zrobienia',
            'text-warning': task.status === 'W trakcie',
            'text-success': task.status === 'Zrobione'
            }">
            {{ task.status }}
            </span>
          </td>
          <td>
            <div class="text-center" @click="editTask(index)">
              <span class="fa fa-pen"></span>
            </div>
          </td>
          <td>
            <div class="text-center" @click="deleteTask(index)">
              <span class="fa fa-trash"></span>
            </div>
          </td>

        </tr>


      </tbody>
    </table>
    <div class=" ">
      <button class="btn btn-primary rounded-0" style="width:312px" @click="changeBgColorTodos(isClickedTodos)" :class="[isClicked /*? 'greenDiv' : 'blueDiv'*/]">Wybierz wszystkie do zrobienia</button>
    <button class="btn btn-secondary rounded-0" style="width:312px" @click="changeBgColorInProgress(isClickedInProgress)" :class="[isClicked]">Wybierz wszystkie w trakcie</button>
    <button class="btn btn-primary rounded-0" style="width:312px" @click="changeBgColorFinished(isClickedFinished)" :class="[isClicked]">Wybierz wszystkie skończone</button>
    </div>
   
  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  props: {
    msg: String
  },
  data() {
    return {
      isClickedTodos: true,
      isClickedInProgress: true,
      isClickedFinished: true,
      task: '',
      editedTask: null,
      availableStatuses: ['Do zrobienia', 'W trakcie', 'Zrobione'],
      tasks: [
        {
          name: 'Do the laundry',
          status: 'Do zrobienia'
        },
        {
          name: 'Make dinner',
          status: 'Do zrobienia'
        },
        {
          name: 'Cook meth',
          status: 'W trakcie'
        }
        

      ]
    }
  },
  methods: {
    submitTask() {
      if (this.task.length === 0) return;

      if (this.editedTask === null) {
        this.tasks.push({
          name: this.task,
          status: 'Do zrobienia'
        })
      } else {
        this.tasks[this.editedTask].name = this.task
        this.editedTask = null;
      }

      this.task = ''
    },
    deleteTask(index) {
      this.tasks.splice(index, 1)
    },
    editTask(index) {
      this.task = this.tasks[index].name
      this.editedTask = index
    },
    changeStatus(index) {
      let newIndex = this.availableStatuses.indexOf(this.tasks[index].status)

      if (++newIndex > 2) newIndex = 0;
      this.tasks[index].status = this.availableStatuses[newIndex]
    },
    changeBgColorTodos(value) {
      this.isClickedTodos = !value
    },
    changeBgColorInProgress(value){
      this.isClickedInProgress = !value
    },
    changeBgColorFinished(value){
      this.isClickedFinished = !value
    }
  }
}



</script>
<style scoped>
.finished {
  text-decoration: line-through;
}
.todoMark{
  background-color: rgb(255, 116, 116);
}
.inProgressMark{
  background-color: rgb(253, 243, 93)
}
.finishedMark{
  background-color: rgb(54, 255, 47);
}
</style>