<template>
  <div class="container">
    <h2 class="text-center mt-5">My Vue Todo App</h2>

    <div class="d-flex">
      <input v-model="task" type="text" placeholder="Enter task" class="form-control">
      <input v-model="task1" type="date" class="form-control">
      <button @click="submitTask" class="btn btn-warning rounded-0">Submit</button>
    </div>

    <!-- Task table -->
    <table class="table table-bordered mt-5">
      <thead>
        <tr>
          <th scope="col">Task</th>
          <th scope="col">Date</th>
          <th scope="col">Status</th>
          <th scope="col" class="text-center">#</th>
          <th scope="col" class="text-center">#</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(task, index) in tasks" :key="index">
          <td>{{ task.name }}</td>
          <td>{{ task.Date }}</td>
          <td>
            <div>
              <input type="checkbox" :id="'checkbox-' + index" v-model="tasks[index].status" @change="updateStatus(index)">
              <label :for="'checkbox-' + index"></label>
            </div>
          </td>
          <td>
            <div @click="editTask(index)">
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
  name: 'HelloWorld',
  props: {
    msg: String
  },
  data() {
    return {
      task: '',
      task1: '', // Added for date input
      editedTask: null, // Track the edited task
      // Retrieve tasks from local storage or use default tasks if local storage is empty
      tasks: JSON.parse(localStorage.getItem('tasks')) || [
        {
          name: "Wake up Early",
          status: false, // Changed to boolean value
          Date: "2024-03-11"
        },
        {
          name: "Drink enough water.",
          status: false, // Changed to boolean value
          Date: "2024-03-12"
        },
        {
          name: "Study for board exam.",
          status: false, // Changed to boolean value
          Date: "2024-03-13"
        }
      ]
    };
  },
  methods: {
    submitTask() {
      if (this.task.length === 0) return;

      if (this.editedTask != null) {
        this.tasks[this.editedTask].name = this.task;
        this.editedTask = null;
      } else {
        // Push new task to tasks array
        this.tasks.push({
          name: this.task,
          Date: this.task1,
          status: false // Changed to boolean value
        });
      }

      // Save tasks array to local storage
      localStorage.setItem('tasks', JSON.stringify(this.tasks));

      // Reset task inputs after submission
      this.task = '';
      this.task1 = '';
    },
    deleteTask(index) {
      this.tasks.splice(index, 1);
      localStorage.setItem('tasks', JSON.stringify(this.tasks));
    },
    editTask(index) {
      this.task = this.tasks[index].name;
      this.editedTask = index;
    },
    updateStatus() {
      // Save tasks array to local storage after updating status
      localStorage.setItem('tasks', JSON.stringify(this.tasks));
    }
  }
};
</script>

<style scoped>
</style>
