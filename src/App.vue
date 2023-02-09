<template>
  <main>
    <!-- heading -->
    <header>
      <img src="./assets/pinia-logo.svg" alt="logo">
      <h1>Pinia Tasks</h1>
    </header>

    <!-- new task form -->
    <div class="new-task-form">
      <TaskForm />
    </div>

    <!-- filter -->
    <nav class="filter">
      <button @click="filter = 'all'">All tasks</button>
      <button @click="filter = 'favs'">Fav tasks</button>
    </nav>

    <!-- loading -->
    <div class="loading" v-if="taskStore.isLoading">
      loading tasks...
    </div>

    <!-- task list -->
    <div class="task-list" v-if="filter === 'all'">
      <p>you have {{ taskStore.tasks.length }} tasks left to do</p>
      <div v-for="task in taskStore.tasks" :key="task.id">
        <TaskDetails :task="task" />
      </div>
    </div>

    <div class="task-list" v-if="filter === 'favs'">
      <p>you have {{ taskStore.favs.length }} fav tasks</p>
      <div v-for="task in taskStore.favs" :key="task.id">
        <TaskDetails :task="task" />
      </div>
    </div>

  </main>
</template>

<script>
  import { ref, onMounted } from 'vue';
  import TaskDetails from "./components/TaskDetails.vue";
  import {useTaskStore} from "./store/TaskStore";
  import TaskForm from './components/TaskForm.vue';

  export default {
    components: {
      TaskDetails,
      TaskForm
    },
    setup () {
      const taskStore = useTaskStore();

      const filter = ref('all');

      onMounted(() => {
        taskStore.getTasks();
      });

      return {
        taskStore,
        filter
      }
    }
  }
</script>
