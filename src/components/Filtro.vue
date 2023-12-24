<template>
  <!-- Contenedor principal del componente Filtro -->
  <div class="py-2 px-10 flex flex-col md:flex-row justify-between items-center">
    <!-- Menú desplegable para seleccionar barrio -->
    <div class="mb-4 md:mb-0">
      <select class="ml-2 bg-gray-800 text-white p-2 pr-2 rounded" v-model="selectedBarrio" @change="filterApartments">
        <!-- Opción por defecto -->
        <option value="todos">Seleccionar Barrio</option>
        <!-- Iteración sobre la lista de barrios -->
        <option v-for="barrio in barrios" :key="barrio.id" :value="barrio.name">{{ barrio.name }}</option>
      </select>
    </div>

    <!-- Botón para abrir el modal de filtros -->
    <button @click="openFilterModal" class="bg-gray-800 text-white px-4 py-2 rounded-md mt-4 md:mt-0">Filtros</button>
  </div>
</template>

<script>
export default {
  data() {
    return {
      selectedBarrio: 'todos', // Barrio seleccionado por defecto
    };
  },
  // Propiedades recibidas desde el componente padre
  props: {
    barrios: Array,
  },
  methods: {
    // Emitir evento al cambiar la selección del barrio
    filterApartments() {
      this.$emit('barrio-selected', this.selectedBarrio);
    },
    
    // Emitir evento al hacer clic en el botón de filtros
    openFilterModal() {
      this.$emit('open-modal');
    },
  },
};
</script>
