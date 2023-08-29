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

/* Styles de l'input dans tous les cas : */
input {
  border: none;
  border-radius: var(--small-border-radius);
  padding: var(--small-padding);
}

/* Styles de l'input dans le cas d'un champ texte : */
input[type='text'] {
  background-color: var(--white-color);
  color: var(--dark-blue-color);
  box-shadow: var(--small-inner-shadow);
}

/* Styles de l'input dans le cas d'un bouton : */
input[type='button'] {
  background-color: var(--dark-blue-color);
  color: var(--white-color);
  box-shadow: var(--small-shadow);
  cursor: pointer;
}

</style>