<template>

  <input
      v-if="props.type === 'text'"
      class="text-input"
      :type="props.type">

  <input
      v-else-if="props.type === 'button'"
      class="button-input"
      :type="props.type"
      :value="props.value">

</template>

<script setup>

/**
 * Composant "Input" :
 * Ce composant représente une balise HTML <input> qui peut être de type "text" ou "button".
 * Pour être utilisé, ce composant doit obligatoirement recevoir l'attribut "type", qui ne peut avoir comme valeur que "text" ou "button".
 */

// Définition des props :
const props = defineProps({

  // Attribut HTML "type" de l'input.
  type: {
    type: String,
    required: true
  },

  // Attribut HTML "value" de l'input dans le cas d'un bouton.
  value: String

});

</script>

<style scoped>

/* Styles dans tous les cas : */
input {
  border: none;
  border-radius: var(--small-border-radius);
  padding: var(--small-padding);
}

/* Styles dans le cas d'un champ texte : */
.text-input {
  background-color: var(--white-color);
  color: var(--dark-blue-color);
  box-shadow: var(--small-inner-shadow);
}

/* Styles dans le cas d'un bouton : */
.button-input {
  background-color: var(--dark-blue-color);
  color: var(--white-color);
  box-shadow: var(--small-shadow);
}

</style>