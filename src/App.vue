<template>
  <div id="app">
    <h1>Tarefas</h1>
    <TasksProgress :progress="progress" />
    <NewTask @taskAdded="addTask" />
    <TaskGrid
      :tasks="tasks"
      @taskDeleted="deleteTask"
      @taskStateChanged="toggleTaskState"
    />
  </div>
</template>

<script>
import TaskGrid from './components/TaskGrid.vue';
import NewTask from './components/NewTask.vue';
import TasksProgress from './components/TasksProgress.vue';

export default {
  components: { TaskGrid, NewTask, TasksProgress },
  data() {
    return {
      tasks: [],
    };
  },
  computed: {
    progress() {
      const total = this.tasks.length;
      const done = this.tasks.filter(task => !task.pending).length;
      return Math.round((done / total) * 100) || 0;
    },
  },
  watch: {
    tasks: {
      deep: true,
      handler() {
        localStorage.setItem('tasks', JSON.stringify(this.tasks));
      },
    },
  },
  methods: {
    addTask(taskToAdd) {
      const reallyNew =
        this.tasks.filter(task => task.name === taskToAdd.name).length === 0;
      if (reallyNew) {
        this.tasks.push({
          name: taskToAdd.name,
          pending: taskToAdd.pending || true,
        });
      }
    },
    deleteTask(task) {
      const index = this.tasks.indexOf(task);
      this.tasks.splice(index, 1);
    },
    toggleTaskState(index) {
      this.tasks[index].pending = !this.tasks[index].pending;
    },
  },
  created() {
    const json = localStorage.getItem('tasks');
    const array = JSON.parse(json);
    this.tasks = Array.isArray(array) ? array : [];
  },
};
</script>

<style>
body {
  font-family: 'Lato', sans-serif;
  background: linear-gradient(to right, rgb(22, 34, 42), rgb(58, 96, 115));
  color: #fff;
}

#app {
  display: flex;
  flex: 1;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  /* height: 100vh; */
}

#app h1 {
  margin-bottom: 5px;
  font-weight: 300;
  font-size: 3rem;
  margin-bottom: 50px;
}
</style>
