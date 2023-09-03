<template>

  <input
      v-if="props.type === 'text'"
      :type="props.type"
      :value="props.value"
      @input="emits('update:value', $event.target.value)">

  <input
      v-else-if="props.type === 'submit'"
      :type="props.type"
      :value="props.value">

</template>

<script setup>

/**
 * DÉFINITION DES PROPS :
 */

const props = defineProps({

  // Attribut "type" de l'input.
  type: {
    type: String,
    required: true
  },

  // Attribut "value" de l'input.
  value: {
    type: String,
    required: true
  }

});

/**
 * DÉFINITION DES EMITS :
 */

const emits = defineEmits(['update:value']);

</script>

<style scoped>

/* Styles de l'input dans tous les cas : */
input {
  border: none;
  border-radius: var(--small-border-radius);
  padding: var(--small-padding);
}

/* Styles de l'input dans le cas d'un champ de type texte : */
input[type='text'] {
  background-color: var(--white-color);
  color: var(--dark-blue-color);
  box-shadow: var(--small-inner-shadow);
}

/* Styles de l'input dans le cas d'un bouton de type "submit" : */
input[type='submit'] {
  background-color: var(--dark-blue-color);
  color: var(--white-color);
  box-shadow: var(--small-shadow);
  cursor: pointer;
}

</style>