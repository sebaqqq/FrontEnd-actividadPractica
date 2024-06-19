<template>
  <div class="modal">
    <div class="modal-content">
      <h2 class="titulo">Eliminar Tarea</h2>
      <p>¿Estás seguro de que deseas eliminar esta tarea?</p>
      <form @submit.prevent="deleteTask">
        <div class="buttons">
          <button type="submit" class="btn btn-delete">Eliminar</button>
          <button type="button" @click="close" class="btn btn-close">
            Cerrar
          </button>
        </div>
      </form>
    </div>
  </div>
</template>

<script setup>
import { defineProps, defineEmits } from "vue";
import axios from "../axios";
// Definimos las propiedades que recibe el componente
const props = defineProps({
  task: {
    type: Object,
    required: true,
  },
});

const emit = defineEmits(["close"]);
// Función para eliminar la tarea
const deleteTask = async () => {
  try {
    await axios.delete(`/tasks/${props.task.id}`);
    emit("close");
  } catch (error) {
    console.error(error);
  }
};

console.log("tarea eliminada correctamente: ", props.task.id);

const close = () => {
  emit("close");
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

.buttons {
  margin-top: 10px;
  display: flex;
  justify-content: flex-end;
}

button {
  margin-left: 10px;
  padding: 6px 12px;
  border-radius: 5px;
  border: none;
  background-color: #555;
  color: white;
  cursor: pointer;
}

button:hover {
  background-color: #777;
}

.btn-delete {
  background-color: #dc3545;
}

.btn-close {
  background-color: #555;
}
</style>
