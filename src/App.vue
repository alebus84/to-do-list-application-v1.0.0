<script setup>

import { ref } from "vue";
//import Input from "@/components/Input.vue";
//import ToDoList from "@/components/ToDoList.vue";

const fieldText = ref("");
const theTextIsTooShort = ref(false);
const greatestId = ref(1);
const listOfToDo = ref({notDone: [], done: []});

const checkLocalStorage = () => {
  if (window.localStorage.getItem("greatestId")) {
    console.log("greatestId exist in local storage");
    greatestId.value = Number(window.localStorage.getItem("greatestId"));
  }
  if (window.localStorage.getItem("listOfToDo")) {
    console.log("listOfToDo exist in local storage");
    listOfToDo.value = JSON.parse(window.localStorage.getItem("listOfToDo"));
  }
}
checkLocalStorage();

const handleCreate = (event) => {
  event.preventDefault();
  const length = 5;
  if (fieldText.value.length < length) {
    theTextIsTooShort.value = true;
  } else {
    theTextIsTooShort.value = false;
    listOfToDo.value.notDone.push({id: greatestId.value, name: fieldText.value});
    window.localStorage.setItem("listOfToDo", JSON.stringify(listOfToDo.value));
    greatestId.value++;
    window.localStorage.setItem("greatestId", `${greatestId.value}`);
  }
};

const handleChecked = (event) => {
  event.preventDefault();
  const id = Number(event.target.getAttribute("aria-label"));
  const isChecked = event.target.checked;
  if (isChecked) {
    const toDoChecked = listOfToDo.value.notDone.filter(toDo => toDo["id"] === id)[0];
    listOfToDo.value.notDone = listOfToDo.value.notDone.filter(toDo => toDo !== toDoChecked);
    listOfToDo.value.done.push(toDoChecked);
  } else {
    const toDoUnchecked = listOfToDo.value.done.filter(toDo => toDo["id"] === id)[0];
    listOfToDo.value.done = listOfToDo.value.done.filter(toDo => toDo !== toDoUnchecked);
    listOfToDo.value.notDone.push(toDoUnchecked);
  }
  listOfToDo.value.notDone.sort((currentToDo, nextToDo) => currentToDo["id"] - nextToDo["id"]);
  listOfToDo.value.done.sort((currentToDo, nextToDo) => currentToDo["id"] - nextToDo["id"]);
  window.localStorage.setItem("listOfToDo", JSON.stringify(listOfToDo.value));
};

</script>

<template>
  <div id="wrapper">
    <input type="text" v-model="fieldText">
    <input type="submit" value="Créer la tâche" @click="handleCreate">
    <p class="alert" v-if="theTextIsTooShort">
      Le nom de la tâche doit contenir au moins 5 caractères.
    </p>
    <ul>
      <li v-for="toDo in listOfToDo.notDone" :key="toDo.id">
        <input type="checkbox" :aria-label="toDo.id" @change="handleChecked"> À faire : {{ toDo.name }}
      </li>
    </ul>
    <ul>
      <li v-for="toDo in listOfToDo.done" :key="toDo.id">
        <input type="checkbox" :aria-label="toDo.id" checked="checked" @change="handleChecked"> Faite : {{ toDo.name }}
      </li>
    </ul>
  </div>
</template>

<style scoped></style>