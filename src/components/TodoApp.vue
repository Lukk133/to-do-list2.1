<template>
 
  <div class="container mt-5">
    <h2 class="text-center">To do list</h2>
    <div class="d-flex">
    <input v-model="task" type="text" class="form-control" placeholder="input a task">
    <button class="btn btn-warning rounded-0" @click="submitTask">SUBMIT</button>
  </div>
    <table class="table table-bordered">
  <thead>
    <tr>
      <th scope="col">Task</th>
      <th scope="col">Status</th>
      <th scope="col">#</th>
      <th scope="col">#</th>
    </tr>
  </thead>
  <tbody>
    <tr v-for="(task, index) in tasks" :key="index">
      <td> 
        <span 
          :class="{'finished': task.status === 'Finished'}">
          {{ task.name }}
        </span>
      </td>
      <td style="width: 120px"><span  @click="changeStatus(index)" 
        :class="{'text-danger' : task.status === 'To do',
        'text-warning': task.status === 'In progress',
        'text-success': task.status === 'Finished'}"
        >{{ task.status }}
      </span>
    </td>
      <td>
      <div class="text-center" @click="editTask(index)">
        <span class="fa fa-pen"></span>
      </div>
    </td>
      <td>
        <div class="text-center"  @click="deleteTask(index)">
        <span class="fa fa-trash"></span>
      </div>
      </td>
      
    </tr>


  </tbody>
</table>
  </div>

</template>

<script>
export default{
  name: 'HelloWorld',
  props: {
    msg: String
  },
  data(){
    return{
      task: '',
      editedTask: null,
      availableStatuses: ['To do', 'In progress', 'Finished'],
      tasks: [
        {
        name: 'Do laundry',
        status: 'To do'
      },
      {
        name: 'Cook meth',
        status: 'In progress'
      }
    ]
    }
  },
  methods: {
      submitTask(){
        if (this.task.length === 0) return;

        if(this.editedTask === null){
          this.tasks.push({
          name: this.task,
          status: 'to-do'
        })
        } else {this.tasks[this.editedTask].name = this.task
        this.editedTask = null;}

        this.task = ''
      },
      deleteTask(index){
        this.tasks.splice(index, 1)
      },
      editTask(index){
        this.task = this.tasks[index].name
        this.editedTask = index
      },
      changeStatus(index){
        let newIndex = this.availableStatuses.indexOf(this.tasks[index].status)

        if(++newIndex > 2) newIndex = 0;
        this.tasks[index].status = this.availableStatuses[newIndex]

      }
    } 
}



</script>
<style scoped>
.finished{
  text-decoration: line-through;
}
</style>