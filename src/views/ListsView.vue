<template>

  <h2>Lists view</h2>

  <!-- FORMULAIRE DE CRÉATION D'UNE NOUVELLE TÂCHE -->

  <form id="new-to-do-form" @submit.prevent="createToDo">

    <span>Nouvelle tâche</span>

    <input-component
        type="text"
        v-model:value="toDoName"/>

    <input-component
        type="submit"
        value="Créer la nouvelle tâche"/>

  </form>

</template>

<script setup>

/**
 * DÉFINITION DES IMPORTATIONS :
 */

import { ref } from "vue";

import InputComponent from "@/components/InputComponent.vue";

/**
 * DÉFINITION DES REFS :
 */

// Champ texte contenant le nom de la nouvelle tâche.
const toDoName = ref('');

/**
 * DÉFINITION DES ÉVÈNEMENTS :
 */

// Fonction qui gère l'envoi du formulaire pour la création de la nouvelle tâche :
const createToDo = () => {
  console.log(toDoName.value);
};

</script>

<style scoped></style>