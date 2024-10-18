<script setup lang="ts">
import { ref } from "vue";

let id: number = 0;

const tareas = ref<
  { id: number; fecha?: number; text: string; completed: boolean, editar: boolean }[]
>([])

const newTarea = ref('');

function add() {
  if (newTarea.value.trim() !== '') {
    tareas.value.push({
      id: id++,
      fecha: Date.now(),
      text: newTarea.value,
      completed: false,
      editar: false,
    });
    newTarea.value = '';
  }
}

function del(id: number) {
  tareas.value = tareas.value.filter((tarea) => tarea.id !== id);
}

function edit(tarea) {
  tarea.editar = !tarea.editar; 
}
</script>

<template>
  <main>
    <div class="container mb-4 flex flex-col">
      <label for="tarea" class="task mb-4 flex"> Ingresar tareas </label>
      <div class="flex flex-row gap-2">
        <input
          type="text"
          class="add flex w-auto"
          id="tarea"
          placeholder="Agrega una tarea"
          required
          v-model="newTarea"
        />
        <button
          @click="add"
          class="rounded border border-white bg-[#0EAE44] p-2 text-white"
        >
          Añadir
        </button>
      </div>
    </div>

    <section class="lista border border-white p-2" v-if="tareas.length > 0">
      <h1 class="mb-4">Tareas por hacer</h1>

      <div class="mb-2 grid grid-cols-3 p-2 font-bold">
        <span>Estado</span>
        <span>Nombre de la tarea</span>
        <span>Acciones</span>
      </div>

      <ul class="grid gap-2">
        <li
          v-for="tarea in tareas"
          :key="tarea.id"
          class="grid grid-cols-3 items-center bg-white p-2 text-black"
        >
          <div class="flex items-center">
            <input type="checkbox" v-model="tarea.completed" />
          </div>
          <input
          v-if="tarea.editar"
          type="text"
          v-model="tarea.text"
          class="border p-1"
          @blur="tarea.editar = false"  
        />
        <span v-else :class="{ completed: tarea.completed }">{{ tarea.text }}</span>          
        <div class="flex gap-2">
            <button
              class="rounded border border-white bg-red-700 p-2 text-white"
              @click="del(tarea.id)"
            >
              Eliminar
            </button>
            <button class="rounded border border-white bg-blue-700 p-2 text-white" @click="edit(tarea)"> {{ tarea.editar ? 'Guardar' : 'Editar' }} </button> 
          </div>
        </li>
      </ul>
    </section>
  </main>
</template>


<style scoped>
.completed {
  text-decoration: line-through;
}

input {
  color: black;
}
</style>
