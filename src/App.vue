<script setup>

import { ref } from "vue";
//import Input from "@/components/Input.vue";
//import ToDoList from "@/components/ToDoList.vue";

const text = ref("");
const theTextIsTooShort = ref(false);
const list = ref([{name: "to do 1", done: false}, {name: "to do 2", done: false}]);

const checkLocalStorage = () => {
  if (window.localStorage.getItem("toDoList")) {
    console.log("toDoList existe dans le localStorage");
    console.log(window.localStorage.getItem("toDoList"));
  } else {
    console.log("toDoList n'existe pas dans le localStorage");
    console.log(window.localStorage.getItem("toDoList"));
  }
}
checkLocalStorage();

const handleCreate = (event) => {
  event.preventDefault();
  const length = 5;
  if (text.value.length < length) {
    theTextIsTooShort.value = true;
  } else {
    theTextIsTooShort.value = false;
    list.value.unshift({name: text.value, done: false});
  }
}

</script>

<template>
  <div id="wrapper">
    <input type="text" v-model="text">
    <input type="submit" value="Créer la tâche" @click="handleCreate">
    <p class="alert" v-if="theTextIsTooShort">
      Le nom de la tâche doit contenir au moins 5 caractères.
    </p>
    <ul>
      <li v-for="toDo in list"><input type="checkbox">{{ toDo.name }}</li>
    </ul>
  </div>
</template>

<style scoped></style>