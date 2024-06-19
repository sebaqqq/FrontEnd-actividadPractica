<template>
  <div class="container dark-mode">
    <h1 class="title">Tareas</h1>
    <button @click="openAddTask" class="btn-add">Añadir Tarea</button>
    <div class="list-container">
      <ul class="task-list">
        <li v-for="task in tasks" :key="task.id" class="task-item">
          <div class="task-header">
            <h2 class="task-title">Tarea: {{ task.title }}</h2>
            <div class="task-buttons">
              <button @click="openEditTask(task)" class="btn-edit">
                Editar
              </button>
              <button @click="openDeleteTask(task)" class="btn-delete">
                Eliminar
              </button>
            </div>
          </div>
          <p>Descripcion: {{ task.description }}</p>
        </li>
      </ul>
    </div>
    <AddTask v-if="showAddTask" @close="closeAddTask" />
    <EditTask v-if="showEditTask" :task="selectedTask" @close="closeEditTask" />
    <DeleteTask
      v-if="showDeleteTask"
      :task="selectedTask"
      @close="closeDeleteTask"
    />
  </div>
</template>

<script setup>
import { ref, onMounted } from "vue";
import axios from "./axios.js";
import AddTask from "./components/AddTask.vue";
import EditTask from "./components/EditTask.vue";
import DeleteTask from "./components/DeleteTask.vue";

// Estado local
const tasks = ref([]);
const showAddTask = ref(false);
const showEditTask = ref(false);
const showDeleteTask = ref(false);
const selectedTask = ref({});

// Función para obtener las tareas
const fetchTasks = async () => {
  try {
    const response = await axios.get("/tasks");
    tasks.value = response.data;
  } catch (error) {
    console.error(error);
  }
};

//Obtener tareas
onMounted(() => {
  fetchTasks();
});

// Funciones para manejar el estado de los modales
const openAddTask = () => {
  showAddTask.value = true;
};

const closeAddTask = () => {
  showAddTask.value = false;
  fetchTasks();
};

const openEditTask = (task) => {
  selectedTask.value = task;
  showEditTask.value = true;
};

const closeEditTask = () => {
  showEditTask.value = false;
  fetchTasks();
};

const openDeleteTask = (task) => {
  selectedTask.value = task;
  showDeleteTask.value = true;
};

const closeDeleteTask = () => {
  showDeleteTask.value = false;
  fetchTasks();
};
</script>

<style>
.container {
  height: 100vh;
  display: flex;
  flex-direction: column;
  background-color: #333;
  color: #eee;
}

.dark-mode {
  background-color: #333;
  color: #eee;
}

.title {
  font-size: 2rem;
  text-align: center;
  margin-top: 20px;
}

.list-container {
  flex: 1;
  overflow-y: auto;
}

.task-list {
  list-style-type: none;
  padding: 0;
}

.task-item {
  margin-bottom: 20px;
  padding: 10px;
  border: 1px solid #555;
  position: relative;
  background-color: #444;
  border-radius: 20px;
  width: 50%;
  margin-left: auto;
  margin-right: auto;
}

.task-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.task-title {
  margin: 0;
}

.task-buttons {
  position: absolute;
  top: 0;
  right: 0;
}

.task-buttons button {
  padding: 8px 16px;
  border-radius: 10px;
  background-color: #555;
  color: #eee;
  cursor: pointer;
  margin-left: 5px;
}

.task-buttons button.btn-edit {
  background-color: #007bff;
}

.task-buttons button.btn-delete {
  background-color: #dc3545;
}

.task-buttons button:hover {
  background-color: #777;
}

.btn-add {
  padding: 8px 16px;
  border-radius: 10px;
  background-color: #28a745;
  color: #eee;
  cursor: pointer;
  margin-top: 20px;
  width: 500px;
  display: block;
  justify-content: center;
  align-items: center;
  margin-left: auto;
  margin-right: auto;
}
</style>
