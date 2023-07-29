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

const getGreatestId = (idsToCheck) => {
  return Math.max(...idsToCheck);
};

const handleCreate = (event) => {
  event.preventDefault();
  /*const stringKeys = Object.keys(listOfToDo.value);
  console.log(stringKeys);
  const numberKeys = [];
  stringKeys.forEach(key => numberKeys.push(Number(key)));
  console.log(numberKeys);*/
  const length = 5;
  if (fieldText.value.length < length) {
    theTextIsTooShort.value = true;
  } else {
    theTextIsTooShort.value = false;
    greatestId.value++;
    window.localStorage.setItem("greatestId", `${greatestId.value}`);
    //console.log(greatestId.value);
    //console.log(Number(window.localStorage.getItem("greatestId")));
    listOfToDo.value.notDone.unshift({id: greatestId.value, name: fieldText.value});
    window.localStorage.setItem("listOfToDo", JSON.stringify(listOfToDo.value));
    //console.log(listOfToDo.value);
    //console.log(JSON.parse(window.localStorage.getItem("listOfToDo")));
    /*const ids = [];
    listOfToDo.value.forEach(toDo => ids.push(toDo.id));
    console.log(ids);
    const greatestId = getGreatestId(ids);
    console.log(greatestId);
    listOfToDo.value.unshift({id: greatestId + 1, name: fieldText.value, done: false});*/
    //window.localStorage.setItem("toDoList", JSON.stringify(listOfToDo.value));
  }
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
      <li v-for="toDo in listOfToDo.notDone"><input type="checkbox" v-bind:checked="toDo.done">{{ toDo.name }}</li>
    </ul>
    <ul>
      <li v-for="toDo in listOfToDo.done"><input type="checkbox" v-bind:checked="toDo.done">{{ toDo.name }}</li>
    </ul>
  </div>
</template>

<style scoped></style>