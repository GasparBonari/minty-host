<template>
    <select v-model="selectedBarrio">
      <option v-for="barrio in barrios" :key="barrio.id" :value="barrio.id">
        {{ barrio.name }}
      </option>
    </select>
  </template>
  
  <script>
  export default {
    props: {
      barrios: {
        type: Array,
        required: true,
      },
      value: {
        type: Number,
        default: null,
      },
    },
    watch: {
      value(newValue) {
        this.selectedBarrio = newValue;
      },
      barrios: {
        handler(newBarrios) {
          // Handle the case when barrios prop changes
          // For example, if you want to reset selectedBarrio when barrios change
          this.selectedBarrio = null;
        },
        immediate: true, // Run the handler immediately when the component is created
      },
      selectedBarrio(newBarrio) {
        this.$emit("input", newBarrio);
      },
    },
    data() {
      return {
        selectedBarrio: this.value,
      };
    },
  };
  </script>
  