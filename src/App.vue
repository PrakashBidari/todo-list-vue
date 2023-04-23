<template>
  <main>

    <!-- headings: -->
    <header>
      <img src="./assets/pinia-logo.svg" alt="pinia logo">
      <h1>{{ taskStore.name }}</h1>
    </header>

    <!-- new task form -->

    <div class="new-task-form">
      <TaskForm />
    </div>

    <!-- filter -->
    <nav class="filter">
      <button @click="filter = 'all'">All tasks</button>
      <button @click="filter = 'favs'">Fevs tasks</button>
    </nav>

    <!-- loading -->
    <div class="loading" v-if="taskStore.loading">Loading tasks...</div>

    <!-- task list -->
    <div class="task-list" v-if="filter === 'all'">
      <p>All tasks {{ taskStore.totalCount }}</p>
      <div v-for="task in taskStore.tasks" :key="task">
        <TaskDetails :task="task" />
      </div>
    </div>

    <div class="task-list" v-if="filter === 'favs'">
      <p>Fev tasks {{ taskStore.favCount }}</p>
      <div v-for="task in taskStore.favs" :key="task">
        <TaskDetails :task="task" />
      </div>
    </div>

    <div class="reset">
      <button  @click="taskStore.$reset">reset state</button>
    </div>
    

  </main>
</template>

<script>
import { ref } from 'vue';
import TaskDetails from './components/TaskDetails.vue'
import TaskForm from './components/TaskForm.vue'
import { useTaskStore } from './stores/TaskStore'
import { storeToRefs } from 'pinia';

export default {
  components: { TaskDetails, TaskForm },
  setup() {
    const taskStore = useTaskStore();

    const { tasks, loading, favs, totalCount, favCount } = storeToRefs(taskStore);

    // fetch tasks
    taskStore.getTasks();

    const filter = ref('all');

    return { taskStore, filter, tasks, loading, favs, totalCount, favCount }
  }
}
</script>
