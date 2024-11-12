<template>
  <div class="container">
    <h2 class="text-center mt-5">My Vue Todo App</h2>

    <div class="input-group mb-3">
      <input
        type="text"
        class="form-control"
        placeholder="Enter New Task"
        v-model="newTask"
        @keyup.enter="addTask"
      />
      <button class="btn btn-primary" @click="addTask">Add Task</button>
    </div>

    <!-- Task table  part -->
    <table class="table table-bordered">
      <thead>
        <tr>
          <th scope="col">#</th>
          <th scope="col">Main Task</th>
          <th scope="col">Subtasks</th>
          <th scope="col">Action</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(task, index) in tasks" :key="task.id">
          <th scope="row">{{ index + 1 }}</th>
          <td>{{ task.name }}</td>
          <td>
            <ul>
              <li
                v-for="(subtask, subIndex) in task.subtasks"
                :key="subtask.id"
              >
                {{ subtask }}
                <!-- index of  main task  -->
                <button
                  class="btn btn-danger btn-sm my-2"
                  @click="deleteSubtask(mainindex, subIndex)"
                >
                  Delete Subtask
                </button>
              </li>
            </ul>
          </td>
          <td>
            <button
              class="btn btn-secondary btn-sm me-2"
              @click="addSubtask(index)"
            >
              Add Subtask
            </button>
            <button class="btn btn-danger btn-sm" @click="deleteTask(index)">
              Delete Task
            </button>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
export default {
  data() {
    return {
      tasks: [
        { name: "Go to Shop", subtasks: ["Buy Apples", " Buy Mango"] },
        { name: "Go To Office", subtasks: ["Complete Task"] },
      ],

      newTask: "",
    };
  },
  methods: {
    addTask() {
      if (this.newTask.trim()) {
        this.tasks.push({ name: this.newTask, subtasks: [] });
        this.newTask = "";
      }
    },

    deleteTask(index) {
      this.tasks.splice(index, 1);
    },

    addSubtask(subindex) {
      const subtaskName = prompt("Enter subtask name:");
      if (subtaskName) {
        this.tasks[subindex].subtasks.push(subtaskName);
      }
    },

    deleteSubtask(mainIndex, subIndex) {
      this.tasks[mainIndex].subtasks.splice(subIndex, 1);
    },
  },
};
</script>
