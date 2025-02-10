<template>
  <h1>TODO LIST</h1>
  <p>Gérer vos tâches à réaliser </p>
  <form action="" @submit.prevent="addTask">
    <fieldset role="group" class="container">
      <input type="text" placeholder="Ajouter une tâche" v-model="task.name" />
      <button :disabled="task.name == 0">Ajouter</button>
    </fieldset>
  </form>
  <div v-if="allTasks.length == 0"> Vous n'avez pas de tâches à faire :(</div>
  <div v-else>
    <div><input type="checkbox" v-model="hideTasks"> Masquer les tâches complétées</div>
    <div class="grid">
      <ul>
        <li v-for="task in sortTasks()" :key="task.name" class="grid-item"
          :style="{ textDecoration: task.done ? 'line-through' : 'none' }">
          <span>{{ task.name }}</span>
          <span class="footer">
            <input type="checkbox" v-model="task.done" /> 
            <button class="delete">
              <img src="/delete.png" alt="delete" @click="removeTask(task.name)" />
            </button>
          </span>
        </li>
      </ul>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'
const hideTasks = ref(false)
const allTasks = ref([{
  name: 'Se documenter sur Vue 3',
  done: true
},
  {
    name: 'Créer une application avec Vue 3',
    done: false
  },
  {
    name: 'Déployer une application Vue 3',
    done: false
  }])
const task = ref({  
  name: '',   
  done: false 
})
const addTask = () => {
  allTasks.value.push({
    ...task.value
  });
  task.value.name = '';
}
const removeTask = (name) => {
  allTasks.value = allTasks.value.filter(task => task.name !== name)
}
const sortTasks = () => {
  const sortedTasks = allTasks.value.toSorted((a, b) => a.done > b.done ? 1 : -1)  
  return hideTasks.value ? sortedTasks.filter(task => !task.done) : sortedTasks
}
</script>

<style>
h1 {
  min-height: 70px;
  text-align: center;
  color: aliceblue;
  background-color: rgb(0, 119, 119);
}

.grid {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 10px;
}

.grid-item {
  display: flex;
  flex-direction: column;
  padding: 15px;
  border: 2px solid #ccc;
  background-color: rgb(194, 245, 247);
  border-radius: 8px;
  min-height: 120px;
  width: 100%;
  box-sizing: border-box;
}
.footer {
  display: flex;
  justify-content: space-between;
  align-items: center;
  gap: 10px;
}
.delete {
  display: flex;
  background-color: transparent;
  border: none;
  cursor: pointer;
  max-width: 60px;
  margin: 0;

}
.delete img {
  width: 20px;
  height: 20px;
}
.grid-item input {
  margin: 0;
}
</style>

