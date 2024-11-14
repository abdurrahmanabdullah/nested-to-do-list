<template>
  <div class="container">
    <h2 class="text-center mt-5">My Vue Todo App</h2>

    <div class="input-group mb-3">
      <input
        type="text"
        class="form-control"
        placeholder="Enter New Task here "
        v-model="newTask"
        @keyup.enter="addTask"
      />
      <button class="btn btn-primary" @click="addTask">Add task</button>
    </div>

    <!--  table part  -->
    <table class="table table-bordered">
      <thead>
        <tr>
          <th scope="col">#</th>
          <th scope="col">Task</th>
          <th scope="col">Subtasks</th>
          <th scope="col">Actions</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(task, index) in tasks" :key="index">
          <th scope="row">{{ index + 1 }}</th>

          <!-- Task part -->
          <td>
            <div>{{ task.name }}</div>

            <!-- Nested Tasks under task -->
            <ul v-if="task.nestedTasks.length">
              <li
                v-for="(nestedTask, nestedIndex) in task.nestedTasks"
                :key="nestedIndex"
              >
                {{ nestedTask.name }}
                <button
                  class="btn btn-danger btn-sm my-2"
                  @click="deleteNestedtask(index, nestedIndex)"
                >
                  Delete nested task
                </button>
              </li>
            </ul>

            <!-- Button to add nested task -->
            <button
              class="btn btn-secondary btn-sm"
              @click="addNestedTask(index)"
            >
              Add Nested Task
            </button>
          </td>

          <!-- Subtask section -->
          <td>
            <ul>
              <li v-for="(subtask, subIndex) in task.subtasks" :key="subIndex">
                {{ subtask.name }}
                <button
                  class="btn btn-danger btn-sm my-2"
                  @click="deleteSubtask(index, subIndex)"
                >
                  Delete Subtask
                </button>
                <!-- Nested subtasks -->
                <ul v-if="subtask.subtasks.length">
                  <li
                    v-for="(nestedSubtask, nestedIndex) in subtask.subtasks"
                    :key="nestedIndex"
                  >
                    {{ nestedSubtask.name }}
                    <button
                      class="btn btn-danger btn-sm my-2"
                      @click="deleteNestedSubtask(index, subIndex, nestedIndex)"
                    >
                      Delete Nested Subtask
                    </button>
                  </li>
                </ul>
                <button
                  class="btn btn-secondary btn-sm"
                  @click="addNestedSubtask(index, subIndex)"
                >
                  Add Nested Subtask
                </button>
              </li>
            </ul>
            <button class="btn btn-secondary btn-sm" @click="addSubtask(index)">
              Add Subtask
            </button>
          </td>

          <!-- Task Actions portion  -->
          <td>
            <button class="btn btn-danger btn-sm" @click="deleteTask(index)">
              Delete all Task
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
      tasks: [],
      newTask: "",
    };
  },
  created() {
    const savedTasks = localStorage.getItem("tasks");
    if (savedTasks) {
      this.tasks = JSON.parse(savedTasks);
    }
  },
  methods: {
    saveTasks() {
      localStorage.setItem("tasks", JSON.stringify(this.tasks));
    },

    addTask() {
      if (this.newTask.trim()) {
        this.tasks.push({ name: this.newTask, subtasks: [], nestedTasks: [] });
        this.newTask = "";
        this.saveTasks();
      }
    },

    deleteTask(index) {
      this.tasks.splice(index, 1);
      this.saveTasks();
    },

    addSubtask(index) {
      const subtaskName = prompt("Enter subtask name please :");
      if (subtaskName) {
        this.tasks[index].subtasks.push({ name: subtaskName, subtasks: [] });
        this.saveTasks();
      }
    },

    addNestedSubtask(parentIndex, subIndex) {
      const subtaskName = prompt("Enter nested subtask name:");
      if (subtaskName) {
        this.tasks[parentIndex].subtasks[subIndex].subtasks.push({
          name: subtaskName,
          subtasks: [],
        });
        this.saveTasks();
      }
    },

    addNestedTask(parentIndex) {
      const taskName = prompt("Enter nested task name please  :");
      if (taskName) {
        this.tasks[parentIndex].nestedTasks.push({
          name: taskName,
          subtasks: [],
          nestedTasks: [],
        });
        this.saveTasks();
      }
    },

    deleteSubtask(parentIndex, subIndex) {
      this.tasks[parentIndex].subtasks.splice(subIndex, 1);
      this.saveTasks();
    },

    deleteNestedSubtask(parentIndex, subIndex, nestedIndex) {
      if (nestedIndex === null) {
        // Delete first level  subtask
        this.tasks[parentIndex].subtasks.splice(subIndex, 1);
      } else {
        // Delete second level subtask
        this.tasks[parentIndex].subtasks[subIndex].subtasks.splice(
          nestedIndex,
          1
        );
      }
      this.saveTasks();
    },

    deleteNestedtask(parentIndex, nestedIndex) {
      this.tasks[parentIndex].nestedTasks.splice(nestedIndex, 1);
      this.saveTasks();
    },
  },
};
</script>

<style scoped>
.container {
  margin-top: 20px;
}
</style>
