<template>
  <li>
    <div>
      {{ task.name }}
      <button
        class="btn btn-danger btn-sm my-2"
        @click="$emit('delete-task', task.id)"
      >
        Delete
      </button>
      <button
        class="btn btn-secondary btn-sm"
        @click="$emit('add-subtask', task.id)"
      >
        Add Subtask
      </button>
    </div>
    <ul v-if="task.subtasks.length">
      <task-item
        v-for="subtask in task.subtasks"
        :key="subtask.id"
        :task="subtask"
        @delete-task="$emit('delete-task', $event)"
        @add-subtask="$emit('add-subtask', $event)"
      ></task-item>
    </ul>
  </li>
</template>

<script>
export default {
  name: "TaskItem",
  props: {
    task: Object,
  },
};
</script>

<style scoped>
li {
  margin: 10px 0;
}
</style>
