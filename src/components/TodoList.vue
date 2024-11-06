<template>
  <div class="task-list">
    <h1>Lista de Tareas</h1>

    <!-- Filtro de tareas -->
    <div class="mb-3">
      <button class="btn btn-secondary" @click="filter = 'all'">Todas</button>
      <button class="btn btn-success" @click="filter = 'completed'">
        Completadas
      </button>
      <button class="btn btn-warning" @click="filter = 'pending'">
        Pendientes
      </button>
    </div>

    <!-- Formulario para crear nuevas tareas -->
    <form @submit.prevent="createTask">
      <div class="form-group">
        <label for="task">Nueva tarea:</label>
        <input
          type="text"
          v-model="newTask"
          class="form-control"
          placeholder="Escribe una tarea"
        />
      </div>
      <button type="submit" class="btn btn-primary mt-2">Agregar tarea</button>
    </form>

    <!-- Lista de tareas -->
    <ul class="list-group mt-3">
      <li
        v-for="(task, index) in filteredTasks"
        :key="task.text"
        class="list-group-item d-flex justify-content-between align-items-center"
      >
        <div>
          <input
            type="checkbox"
            class="me-2"
            :checked="task.completed"
            @change="toggleComplete(task)"
          />
          <span :class="{ completed: task.completed }">{{ task.text }}</span>
        </div>
        <button class="btn btn-danger btn-sm" @click="deleteTask(index)">
          Eliminar
        </button>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  data() {
    return {
      newTask: "", // Variable para la nueva tarea
      tasks: [], // Array de tareas
      filter: "all", // Filtro de tareas, por defecto es 'all'
    };
  },
  methods: {
    // Crear una nueva tarea
    createTask() {
      if (this.newTask.trim()) {
        this.tasks.push({
          text: this.newTask,
          completed: false,
        });
        this.newTask = ""; // Limpiar input después de añadir
      }
    },
    // Marcar tarea como completada o no
    toggleComplete(task) {
      task.completed = !task.completed;
    },
    // Eliminar tarea
    deleteTask(index) {
      this.tasks.splice(index, 1);
    },
  },
  computed: {
    // Filtrar las tareas según el filtro seleccionado
    filteredTasks() {
      if (this.filter === "completed") {
        return this.tasks.filter((task) => task.completed);
      } else if (this.filter === "pending") {
        return this.tasks.filter((task) => !task.completed);
      }
      return this.tasks; // Si el filtro es 'all', devolver todas las tareas
    },
  },
};
</script>

<style scoped>
.task-list {
  max-width: 600px;
  margin: 0 auto;
}
.completed {
  text-decoration: line-through;
  color: gray;
}
</style>
