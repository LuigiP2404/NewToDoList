<template>
  <header>
    <h1>To-Do List</h1>
  </header>
  <section id="todo-list" v-cloak>
    <h2>To-Do</h2>
    <input type="text" v-model="newTask" @keyup.enter="addTask" />
    <button @click="addTask">Add</button>
    <div :style="buttonStyle">
      <todo-item
        v-for="(task, id) in tasks"
        :key="id"
        :todoTask="task"
        @removeTaskEmit="removeTask"
        >{{ id }}: {{ task }}</todo-item
      >
    </div>
    <button @click="toggleList">{{ buttonText }}</button>
  </section>
</template>

<script>
import todoItem from "./components/todoItem.vue";
import "@/assets/css/styles.css";

export default {
  name: "App",
  components: {
    todoItem,
  },
  data() {
    return {
      tasks: [],
      newTask: "",
      show: true,
    };
  },
  computed: {
    buttonText() {
      if (this.show) {
        return "Hide List";
      } else {
        return "Show List";
      }
    },
    buttonStyle() {
      if (this.show) {
        return {
          visibility: "inherit",
        };
      } else {
        return {
          visibility: "hidden",
        };
      }
    },
  },
  methods: {
    addTask() {
      // When an item is added, add in both local storage and tasks array
      let taskObj = {
        taskId: this.tasks.length,
        taskText: this.newTask,
      };
      this.tasks.push(taskObj);
      window.localStorage.setItem("task_" + taskObj.taskId, taskObj.taskText);
      this.newTask = "";
    },
    toggleList() {
      this.show = !this.show;
    },
    removeTask(idTask) {
      // Once an item is deleted, get the index of the item whose taskId = the given taskId (by the emit), remove that from both local storage and tasks array
      let index = this.tasks.map(function(e) { return e.taskId; }).indexOf(idTask);
      this.tasks.splice(index, 1);
      window.localStorage.removeItem("task_" + idTask);
    },
  },
  mounted() {
    // Once the page is loaded, check the local storage and push every item into tasks array, so they will be rendered in the page
    let localStorageLength = window.localStorage.length;
    for (let i = 0; i < localStorageLength; i++) {
      let localStorageItem = localStorage.key(i);
      if (localStorageItem.includes("task_")) {
        let obj = {
          taskId: localStorageItem.replace("task_", ""),
          taskText: localStorage.getItem(localStorageItem),
        };
        this.tasks.push(obj);
      }
    }
  },
};
</script>

<style></style>
