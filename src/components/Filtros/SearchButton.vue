<template>
    <button @click="search">Search</button>
  </template>
  
  <script>
  export default {
    methods: {
      search() {
        this.$emit("click");
      },
    },
  };
  </script>