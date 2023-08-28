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

  // Attribut "type" de l'input.
  type: {
    type: String,
    required: true
  },

  // Attribut "value" de l'input.
  value: {
    type: String,
    required: false
  }

});

</script>

<style scoped>

.text-input {
  background-color: var(--white-color);
  color: var(--dark-blue-color);
  border: none;
  box-shadow: var(--small-inner-shadow);
  border-radius: var(--small-border-radius);
  padding: var(--small-padding);
}

.button-input {
  background-color: var(--dark-blue-color);
  color: var(--white-color);
  border: none;
  box-shadow: var(--small-shadow);
  border-radius: var(--small-border-radius);
  padding: var(--small-padding);
}

</style>