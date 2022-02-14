<template>
  <header>
    <h1>To-Do List</h1>
  </header>
  <section id="todo-list" v-cloak>
    <h2>To-Do</h2>
    <input type="text" v-model="newTask" @keyup.enter="addTask">
    <button @click="addTask">Aggiungi</button>
    <div v-if="show">
      <todo-item v-for="(task,id) in tasks" :key="task" :todoTask="task" @removeTaskEmit="removeTask(id)">{{ id + 1 }}: {{ task }}</todo-item>
    </div>
    <button @click="toggleList">{{ fraseBottone }}</button>
  </section>
</template>

<script>
import todoItem from './components/todoItem.vue'
import "@/assets/css/styles.css";

export default {
  name: 'App',
  components: {
    todoItem
  },
  data() {
        return {
            tasks: [
              {
                taskId: '',
                taskText: ''
              }
            ],
            newTask: '',
            show: true
        };
    },
    computed: {
        fraseBottone() {
            if (this.show) {
                return 'Nascondi Lista';
            }
            else {
                return 'Mostra Lista';
            }
        }
    },
    methods: {
        addTask() {
            let taskObj = {
              taskId: this.tasks.length + 1,
              taskText: this.newTask
            }
            this.tasks.push(taskObj);
            window.localStorage.setItem(taskObj.taskId, taskObj.taskText);
            this.newTask = '';
        },
        toggleList() {
            this.show = !this.show;
        },
        removeTask(id) {
            this.tasks.splice(id, 1);
        }
    }
}
</script>

<style>

</style>
