<template>
  <div class="container">
    <h1 class="text">Todo List App</h1>

    <!-- input -->
    <div class="d-flex">
      <input
        v-model="task"
        type="text"
        placeholder="Enter task"
        class="form-control"
      />
      <button @click="submitTask" class="btn btn-warning rounded-0">
        ADD TASK
      </button>
    </div>

    <!-- Task table -->
    <table class="table table-bordered">
      <thead>
        <tr>
          <th scope="col">Task</th>
          <th scope="col">Status</th>
          <th scope="col" class="text-center">#</th>
          <th scope="col" class="text-center">#</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(task, index) in tasks" :key="index">
          <th>
            <span :class="{ 'finished': task.status === 'finished' }">{{
              task.name
            }}</span>
          </th>
          <td style="width: 120px">
            <span
              @click="changeStatus(index)"
              class="pointer"
              :class="{
                'text-danger': task.status === 'to-do',
                'text-warning': task.status === 'in-progress',
                'text-success': task.status === 'finished',
              }"
              >{{ firstCharUpper(task.status) }}</span
            >
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
  </div>
</template>

<script>
export default {
  name: "TodoApp",
  props: {
    msg: String,
  },

  data() {
    return {
      task: "",
      editedTask: null,
      availableStatuses: ["to-do", "in-progress", "finished"],
      tasks: [],
    };
  },

  methods: {
    submitTask() {
      if (this.task.length === 0) alert("You haven't written any to do task");

      if (this.editedTask === null) {
        this.tasks.push({
          name: this.task,
          status: "to-do",
        });
      } else {
        this.tasks[this.editedTask].name = this.task;
        this.editedTask = null;
      }

      this.task = "";
    },
    deleteTask(index) {
      this.tasks.splice(index, 1);
    },
    editTask(index) {
      this.task = this.tasks[index].name;
      this.editedTask = index;
    },
    changeStatus(index) {
      let newIndex = this.availableStatuses.indexOf(this.tasks[index].status);
      if (++newIndex > 2) newIndex = 0;
      this.tasks[index].status = this.availableStatuses[newIndex];
    },
    firstCharUpper(str) {
      return str.charAt(0).toUpperCase() + str.slice(1);
    },
  },
};
</script>

<style scoped>
.container {
  max-width: 600px;
  margin: 20px auto;
  padding: 20px;
  background-color: #e9ecef;
  border-radius: 8px;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
}
.text {
  text-align: center;
  color: #007bff;
}
.d-flex{
  display: flex;
  margin-bottom: 20px;
}
.d-flex input{
  flex: 1;
  padding: 8px;
  border: 1px solid #ced4da;
  border-radius: 4px;
}
.d-flex button{
  padding: 8px 16px;
  background-color: #ffc107;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  color: #212529;
}
.table{
  width: 100%;
  border-collapse: collapse;
  margin-bottom: 20px;
}
.table th, td {
  padding: 10px;
  border: 1px solid #ced4da;
  text-align: left;
}
.table th {
  background-color: #007bff;
  color: #fff;
}
.text-center{
  cursor: pointer;
  text-align: center;
}
.text-danger {
  font-weight: bold;
  color: #dc3545; 
}
.text-warning {
  font-weight: bold;
  color: #ffc107; 
}
.text-success {
  font-weight: bold;
  color: #28a745; 
}
.pointer {
  cursor: pointer;
}
.finished {
  text-decoration: line-through;
}
</style>
