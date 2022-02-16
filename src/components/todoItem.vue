<template>
  <div class="todo-item">
    <!-- <p>{{ todoTask.taskId }}:</p> -->
    <p>{{ todoTask.taskText }}</p>
    <i
      class="fas fa-trash"
      style="color: crimson"
      @click="removeTaskEmit(todoTask.taskId)"
    ></i>
  </div>
</template>

<script>
export default {
  name: "todoItem",
  props: {
    todoTask: Object,
  },
  emits: ["removeTaskEmit"],
  methods: {
    removeTaskEmit(id) {
      this.$emit("removeTaskEmit", id);
    },
  },
};
</script>

<style scoped></style>
