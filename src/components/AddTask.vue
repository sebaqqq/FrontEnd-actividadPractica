<template>
  <div class="modal">
    <div class="modal-content">
      <h2 class="titulo">Añadir Tarea</h2>
      <form @submit.prevent="addTask">
        <div class="form-group">
          <label for="title">Título</label>
          <input
            type="text"
            v-model="newTask.title"
            placeholder="Ingrese el título de la tarea"
            class="rounded-input"
            required
          />
        </div>
        <div class="form-group">
          <label for="description">Descripción</label>
          <input
            type="text"
            v-model="newTask.description"
            placeholder="Ingrese la descripción de la tarea"
            class="rounded-input"
            required
          />
        </div>
        <div class="buttons">
          <button type="submit" class="btn btn-save">Añadir</button>
          <button type="button" @click="close" class="btn btn-close">
            Cerrar
          </button>
        </div>
      </form>
    </div>
  </div>
</template>

<script>
import axios from "../axios";

// Exportamos un objeto con las propiedades props y data
export default {
  data() {
    return {
      newTask: {
        title: "",
        description: "",
        status: false,
      },
    };
  },
  methods: {
    // Función para añadir una tarea
    async addTask() {
      try {
        await axios.post("/tasks", this.newTask);
        this.newTask.title = "";
        this.newTask.description = "";
        this.newTask.status = false;
        this.$emit("close");
      } catch (error) {
        console.error(error);
      }
    },
    close() {
      this.$emit("close");
    },
  },
};
</script>

<style scoped>
.modal {
  position: fixed;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  background-color: rgba(0, 0, 0, 0.5);
  width: 100%;
  height: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
}

.modal-content {
  background-color: #333;
  padding: 20px;
  border-radius: 10px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.2);
}

.form-group {
  margin-bottom: 15px;
}

label {
  display: block;
  margin-bottom: 5px;
  color: white;
}

.rounded-input {
  border-radius: 20px;
  padding: 8px;
  background-color: #444;
  color: white;
  border: none;
  width: 100%;
}

.buttons {
  margin-top: 20px;
  display: flex;
  justify-content: flex-end;
}

button {
  padding: 8px 16px;
  border-radius: 5px;
  border: none;
  color: white;
  cursor: pointer;
  margin-left: 10px;
}

button:hover {
  background-color: #555;
}

.btn-save {
  background-color: #007bff;
}

.btn-close {
  background-color: #dc3545;
}
</style>
