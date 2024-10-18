<script setup lang="ts">

import { ref } from "vue";

let id: number = 0;

const tareas = ref([
  {id: id++, fecha: undefined, text: '', completed: false }
]);


const newTarea = ref('');


function add () {
      if (newTarea.value.trim() !== '') {
        tareas.value.push({id: id++, fecha: Date.now(), text: newTarea.value, completed: false });
        newTarea.value = '';  
        }
    };


    function del(id: number) {
  tareas.value = tareas.value.filter((tarea) => tarea.id !== id);

}

</script>

<template>
  <main>
    <div class="container flex flex-col">
      <label for="tarea" class="task flex"> Tarea </label>
      <input
        type="text"
        class="add flex"
        id="tarea"
        placeholder="Agrega una tarea"
        required
        v-model="newTarea"
      />
      <button @click="add"> Añadir </button>
    </div>

    <section class="lista" :v-if="tareas.length > 0">
      <h1> Tareas por hacer </h1>
      <ul>
        <li v-for="tarea in tareas" :key="tarea.id">
          <input type="checkbox" v-model="tarea.completed" >
          <span :class="{ completed: tarea.completed }">{{ tarea.text }}</span> 
          <button class="bg-red-700 text-white" @click="del(tarea.id)">Eliminar
            
          </button>

        </li>
      </ul>
    </section>
  </main>
</template>


<style scoped>

.completed {
  text-decoration: line-through;
  color: rgb(0, 0, 0);
}

</style>
