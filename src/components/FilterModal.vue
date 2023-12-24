<template>
    <!-- Sección principal del template Vue.js -->
    <div>
        <!-- Transición para el modal -->
        <transition name="modal" v-if="showModal">
            <!-- Fondo y contenedor del modal -->
            <div class="fixed inset-0 overflow-y-auto z-30">
                <div class="flex items-center justify-center min-h-screen">
                    <!-- Superposición clickeable para cerrar el modal -->
                    <div class="fixed inset-0 transition-opacity" @click="closeModal">
                        <div class="absolute inset-0 bg-gray-500 opacity-75"></div>
                    </div>
                    <!-- Contenedor del contenido del modal -->
                    <div class="flex flex-col aling-center bg-white rounded-lg overflow-hidden max-w-lg p-6 z-50">
                        <!-- Botón para cerrar el modal -->
                        <button @click="closeModal" class="m-auto mr-0 float-right text-gray-600 cursor-pointer">
                            <!-- Icono de cierre -->
                            <svg class="h-6 w-6" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="https://icons8.com/icon/71200/close">
                                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12"></path>
                            </svg>
                        </button>

                        <!-- Entradas del formulario para filtros -->
                        <div class="mb-4">
                            <label for="numRooms" class="block text-sm font-medium text-gray-700">Número de Habitaciones:</label>
                            <input type="number" id="numRooms" v-model="numRooms" class="mt-1 p-2 border w-full rounded-md" min="1">
                        </div>
                        <div class="mb-4">
                            <label for="numBath" class="block text-sm font-medium text-gray-700">Número de Baños:</label>
                            <input type="number" id="numBath" v-model="numBath" class="mt-1 p-2 border w-full rounded-md" min="1">
                        </div>
                        <div class="mb-4">
                            <label for="maxAccommodates" class="block text-sm font-medium text-gray-700">Capacidad Máxima:</label>
                            <input type="number" id="maxAccommodates" v-model="maxAccommodates" class="mt-1 p-2 border w-full rounded-md" min="1">
                        </div>
                        <div class="mb-4">
                            <label for="minPrice" class="block text-sm font-medium text-gray-700">Precio Mínimo:</label>
                            <input type="number" id="minPrice" v-model="minPrice" class="mt-1 p-2 border w-full rounded-md" min="1">
                        </div>
                        <div class="mb-4">
                            <label for="maxPrice" class="block text-sm font-medium text-gray-700">Precio Máximo:</label>
                            <input type="number" id="maxPrice" v-model="maxPrice" class="mt-1 p-2 border w-full rounded-md" min="1">
                        </div>
            
                        <!-- Botones de búsqueda y reinicio -->
                        <button @click="applyFilters" class="bg-gray-800 text-white px-4 py-2 rounded-md">Buscar</button>
                        <button @click="resetFilters" class="bg-gray-500 text-white px-4 py-2 rounded-md mt-4">Reiniciar</button>
                    </div>
                </div>
            </div>
        </transition>
    </div>
</template>
  
<script>
  export default {
    // Datos del componente
    data() {
        return {
            showModal: false,
            numRooms: null,
            numBath: null,
            maxAccommodates: null,
            minPrice: null,
            maxPrice: null,
        };
    },
    // Métodos del componente
    methods: {
        // Abrir el modal
        openModal() {
            this.showModal = true;
        },
        // Cerrar el modal
        closeModal() {
            this.showModal = false;
        },
        // Aplicar filtros y emitir evento
        applyFilters() {
            this.$emit('filters-applied', {
                numRooms: this.numRooms,
                numBath: this.numBath,
                maxAccommodates: this.maxAccommodates
            });
    
            // Cerrar el modal
            this.closeModal();
        },
        // Reiniciar filtros y emitir evento
        resetFilters() {
            this.numRooms = null;
            this.numBath = null;
            this.maxAccommodates = null;
            this.minPrice = null;
            this.maxPrice = null;

            this.$emit('filters-reset');
            this.closeModal();
        },
    },
  };
</script>
  
<style>
  /* Estilos de transición para el modal */
  .modal-enter-active, .modal-leave-active {
    transition: opacity 0.3s;
  }
  .modal-enter, .modal-leave-to {
    opacity: 0;
  }
</style>
