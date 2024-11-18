<template>
  <div class="container">
    <h2 class="text-center mt-5">My Vue Todo App</h2>

    <div class="input-group mb-3">
      <input
        type="text"
        class="form-control"
        placeholder="Enter New Task here"
        v-model="newTask"
        @keyup.enter="addTask"
      />
      <button class="btn btn-primary" @click="addTask">Add Task</button>
    </div>

    <div>
      <ul>
        <task-item
          v-for="task in tasks"
          :key="task.id"
          :task="task"
          @delete-task="deleteTask"
          @add-subtask="addSubtask"
        ></task-item>
      </ul>
    </div>
  </div>
</template>

<script>
import TaskItem from "./components/TaskDisplay.vue";
import { v4 as uuidv4 } from "uuid";

export default {
  components: {
    TaskItem,
  },
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
      console.log("Loaded tasks from localStorage:", this.tasks);
    }
  },
  methods: {
    saveTasks() {
      console.log("Saving tasks:", JSON.stringify(this.tasks, null, 2));
      localStorage.setItem("tasks", JSON.stringify(this.tasks));
    },
    logHierarchy() {
      console.log(
        "Current Task Hierarchy:",
        JSON.stringify(this.tasks, null, 2)
      );
    },
    addTask() {
      if (this.newTask.trim()) {
        this.tasks.push({
          id: uuidv4(),
          name: this.newTask,
          subtasks: [],
        });
        this.newTask = "";
        this.saveTasks();
        this.logHierarchy();
      }
    },
    addSubtask(parentId) {
      const subtaskName = prompt("Enter subtask name:");
      if (subtaskName) {
        const newSubtask = {
          id: uuidv4(),
          name: subtaskName,
          subtasks: [],
        };
        this.addSubtaskToParent(this.tasks, parentId, newSubtask);
        this.saveTasks();
        this.logHierarchy();
      }
    },
    addSubtaskToParent(tasks, parentId, newSubtask) {
      for (const task of tasks) {
        if (task.id === parentId) {
          task.subtasks.push(newSubtask);
          return;
        }
        if (task.subtasks.length) {
          this.addSubtaskToParent(task.subtasks, parentId, newSubtask);
        }
      }
    },
    deleteTask(taskId) {
      this.deleteTaskById(this.tasks, taskId);
      this.saveTasks();
      this.logHierarchy();
    },
    deleteTaskById(tasks, taskId) {
      const index = tasks.findIndex((task) => task.id === taskId);
      if (index !== -1) {
        tasks.splice(index, 1);
        return true;
      }
      for (const task of tasks) {
        if (task.subtasks.length) {
          const found = this.deleteTaskById(task.subtasks, taskId);
          if (found) return true;
        }
      }
      return false;
    },
  },
};
</script>

<style scoped>
.container {
  max-width: 600px;
  margin: auto;
}
</style>
