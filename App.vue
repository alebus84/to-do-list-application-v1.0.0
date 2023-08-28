<script setup>

import { ref, onMounted, onUpdated, onBeforeMount } from "vue";
import Home from "@/views/Home.vue";

/**
 * REFS :
 */

const inputElement = ref(null);
const submitButton = ref(null);
const fieldText = ref("");
const theTextIsTooShort = ref(false);
const greatestId = ref(1);
const listOfToDo = ref({notDone: [], done: []});

/**
 * HOOKS :
 */

onBeforeMount(() => {
  checkLocalStorage();
})

onMounted(() => {
  inputElement.value.addEventListener("keypress", (event) => {
    if (event.key === "Enter") {
      submitButton.value.click();
    }
  });
  addMouseEvents();
})

onUpdated(() => {
  addMouseEvents();
})

/**
 * COMPORTEMENTS :
 */

const checkLocalStorage = () => {
  if (window.localStorage.getItem("greatestId")) greatestId.value = Number(window.localStorage.getItem("greatestId"));
  if (window.localStorage.getItem("listOfToDo")) listOfToDo.value = JSON.parse(window.localStorage.getItem("listOfToDo"));
}

const addMouseEvents = () => {
  const lis = document.querySelectorAll("li");
  lis.forEach(li => li.addEventListener("mouseenter", (event) => {
    event.target.querySelector("input[type='submit']").classList.replace("hidden", "visible");
  }));
  lis.forEach(li => li.addEventListener("mouseleave", (event) => {
    event.target.querySelector("input[type='submit']").classList.replace("visible", "hidden");
  }));
};

const handleCreate = (event) => {
  event.preventDefault();
  const length = 5;
  if (fieldText.value.length < length) theTextIsTooShort.value = true;
  else {
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

const handleDelete = (event) => {
  event.preventDefault();
  const id = Number(event.target.getAttribute("aria-label"));
  const isChecked = event.target.getAttribute("aria-checked");
  if (isChecked === "true") listOfToDo.value.done = listOfToDo.value.done.filter(toDo => toDo["id"] !== id);
  else listOfToDo.value.notDone = listOfToDo.value.notDone.filter(toDo => toDo["id"] !== id);
  listOfToDo.value.notDone.sort((currentToDo, nextToDo) => currentToDo["id"] - nextToDo["id"]);
  listOfToDo.value.done.sort((currentToDo, nextToDo) => currentToDo["id"] - nextToDo["id"]);
  window.localStorage.setItem("listOfToDo", JSON.stringify(listOfToDo.value));
};

</script>

<template>
  <div id="application-wrapper">

    <Home/>

    <h1>To do list application</h1>
    <div id="form-wrapper">
      <h2>Nouvelle tâche</h2>
      <div id="inputs-wrapper">
        <input ref="inputElement" type="text" v-model="fieldText">
        <input ref="submitButton" type="submit" value="Créer" @click="handleCreate">
      </div>
      <p class="alert" v-if="theTextIsTooShort">
        Le nom de la tâche doit contenir au moins 5 caractères.
      </p>
    </div>
    <div id="not-done-wrapper">
      <h2>À faire</h2>
      <ul>
        <li class="notDone created" v-for="toDo in listOfToDo.notDone" :key="toDo.id" :id="toDo.id">
          <input type="checkbox" :aria-label="toDo.id" @change="handleChecked">
          {{ toDo.name }}
          <input type="submit" :aria-label="toDo.id" class="hidden" aria-checked="false" value="X" @click="handleDelete">
        </li>
      </ul>
    </div>
    <div id="done-wrapper">
      <h2>Fait</h2>
      <ul>
        <li class="done created" v-for="toDo in listOfToDo.done" :key="toDo.id" :id="toDo.id">
          <input type="checkbox" :aria-label="toDo.id" checked="checked" @change="handleChecked">
          <span>{{ toDo.name }}</span>
          <input type="submit" :aria-label="toDo.id" class="hidden" aria-checked="true" value="X" @click="handleDelete">
        </li>
      </ul>
    </div>
  </div>
</template>

<style scoped>

#application-wrapper {
  font-family: Arial, sans-serif;
  background-color: rgb(0, 0, 100);
  box-sizing: border-box;
  border-radius: 3px;
  color: rgb(0, 0, 100);
  padding: 5px;
  display: flex;
  flex-direction: column;
  gap: 5px;
}

#application-wrapper * {
  box-sizing: border-box;
}

#application-wrapper h1 {
  color: rgb(255, 255, 255);
  margin: 0;
  padding: 5px;
  text-align: center;
}

#application-wrapper h2 {
  margin: 0;
  padding: 5px;
  text-align: center;
}

#application-wrapper input {
  color: rgb(0, 0, 100);
  border-radius: 3px;
}

#application-wrapper input[type='text'],
#application-wrapper input[type='checkbox'] {
  background-color: rgb(255, 255, 255);
  border: none;
  box-shadow: inset 2px 2px 5px rgba(0, 0, 0, 0.5);
}

#application-wrapper input[type='submit'],
#application-wrapper input[type='checkbox'] {
  cursor: pointer;
}

#application-wrapper input[type='checkbox'] {
  appearance: none;
  width: 12px;
  height: 12px;
}

#application-wrapper input[type='checkbox']:checked {
  position: relative;
  display: block;
  overflow: hidden;
}

#application-wrapper input[type='checkbox']:checked:after {
  content: "\002714";
  display: block;
  color: rgb(0, 0, 100);
  position: absolute;
  top: 6px;
  left: 1px;
  line-height: 0;
}

#application-wrapper input[type='submit'] {
  font-size: 1em;
  background-color: rgb(0, 0, 100);
  color: rgb(255, 255, 255);
  border: none;
  box-shadow: 2px 2px 5px rgba(0, 0, 0, 0.5);
  flex: none;
  padding: 3px 6px 3px 6px;
}

#form-wrapper {
  background-color: rgb(200, 200, 255);
  border-radius: 3px;
}

#inputs-wrapper {
  padding: 5px;
  display: flex;
  gap: 5px;
  justify-content: center;
}

#form-wrapper p {
  margin: 0;
  padding: 5px;
  text-align: center;
  color: rgb(200, 0, 0);
}

#not-done-wrapper,
#done-wrapper {
  background-color: rgb(200, 200, 255);
  border-radius: 3px;
}

#not-done-wrapper ul,
#done-wrapper ul {
  list-style: none;
  margin: 0;
  padding: 5px;
  display: flex;
  flex-direction: column;
  gap: 5px;
  align-items: center;
}

#not-done-wrapper li,
#done-wrapper li {
  padding: 5px;
  border-radius: 3px;
  box-shadow: 2px 2px 5px rgba(0, 0, 0, 0.5);
  align-items: center;
  gap: 5px;
}

#not-done-wrapper li.notDone {
  background-color: rgb(255, 255, 255);
}

#done-wrapper li.done {
  background-color: rgb(150, 150, 150);
}

#not-done-wrapper li.notDone.created,
#done-wrapper li.done.created {
  display: flex;
  visibility: visible;
  transition:
      display 0s ease 0.5s,
      visibility 0.5s ease 0s;
}

#not-done-wrapper li.notDone.created {
  opacity: 1;
  transition: opacity 0.5s ease 0s;
}

#done-wrapper li.done.created {
  opacity: 0.7;
  transition: opacity 0.5s ease 0s;
}

#done-wrapper li.done.created:hover {
  opacity: 1;
}

#not-done-wrapper li.notDone.deleted,
#done-wrapper li.done.deleted {
  display: none;
  opacity: 0;
  visibility: hidden;
  transition:
      display 0s ease 0.5s,
      opacity 0.5s ease 0s,
      visibility 0.5s ease 0s;
}

#not-done-wrapper li.notDone.deleted {}

#done-wrapper li.done.deleted {}

#done-wrapper li.done.created :not(input) {
  text-decoration: line-through;
}

#not-done-wrapper input[class='hidden'],
#done-wrapper input[class='hidden'] {
  visibility: hidden;
  opacity: 0;
  transition-duration: 0.5s;
  transition-timing-function: ease;
  transition-property: visibility, opacity;
}

#not-done-wrapper input[class='visible'],
#done-wrapper input[class='visible'] {
  visibility: visible;
  opacity: 1;
  transition-duration: 0.5s;
  transition-timing-function: ease;
  transition-property: visibility, opacity;
}

</style>