<template>
  <div class="modal">
    <div class="modal-content">
      <h1>Editar Tarea</h1>
      <form @submit.prevent="editTask">
        <div class="form-group">
          <label for="title">Título de la Tarea</label>
          <input
            v-model="editedTask.title"
            placeholder="Título de la Tarea"
            class="rounded-input"
            required
          />
        </div>
        <div class="form-group">
          <label for="description">Descripción de la Tarea</label>
          <input
            v-model="editedTask.description"
            placeholder="Descripción de la Tarea"
            class="rounded-input"
            required
          />
        </div>
        <div class="buttons">
          <button type="submit" class="btn btn-save">Guardar Cambios</button>
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
  props: {
    task: {
      type: Object,
      required: true,
    },
  },
  data() {
    return {
      editedTask: { ...this.task },
    };
  },
  methods: {
    // Función para editar una tarea
    async editTask() {
      try {
        await axios.put(`/tasks/${this.editedTask.id}`, this.editedTask);
        this.$emit("task-updated", this.editedTask);
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
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.5);
  display: flex;
  justify-content: center;
  align-items: center;
}

.modal-content {
  background-color: #333;
  padding: 20px;
  border-radius: 10px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.2);
  width: 400px;
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

.btn-save {
  background-color: #007bff;
}

.btn-close {
  background-color: #dc3545;
}

button:hover {
  background-color: #555;
}
</style>
