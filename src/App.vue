<template>
  <!-- Contenedor principal de la aplicación Vue.js -->
  <div id="app">
    <!-- Barra de navegación responsive -->
    <NavbarResponsive />
    <!-- Componente de filtro con propiedades y eventos -->
    <Filtro :barrios="barrios" @barrio-selected="handleBarrioSelected" @open-modal="openFilterModal" />
    <!-- Contenedor de tarjetas de apartamentos -->
    <div class="flex flex-wrap justify-center py-10">
      <!-- Iteración sobre apartamentos filtrados para mostrar tarjetas -->
      <ApartmentCard
        v-for="apartment in filteredApartments"
        :key="apartment.id"
        :apartment="apartment"
      />
    </div>

    <!-- Modal de filtros con referencias y eventos -->
    <FilterModal ref="filterModal" @filters-applied="handleFiltersApplied" @filters-reset="handleFiltersReset" />
  </div>
</template>

<script>
import NavbarResponsive from "@/components/NavbarResponsive.vue";
import ApartmentCard from "@/components/ApartmentCard.vue";
import Filtro from "@/components/Filtro.vue";
import FilterModal from "@/components/FilterModal.vue";
import axios from "axios";

export default {
  name: "App",
  components: {
    NavbarResponsive,
    Filtro,
    ApartmentCard,
    FilterModal,
  },
  data() {
    return {
      apartments: [],
      filteredApartments: [],
      barrios: [],
      selectedBarrio: null,
    };
  },
  // Método ejecutado después de que la instancia es creada
  mounted() {
    // Obtener la lista de apartamentos y barrios
    this.fetchApartments();
    this.fetchBarrios();
  },
  methods: {
    // Obtener la lista de barrios desde la API
    async fetchBarrios() {
      try {
        const response = await axios.get('https://api.dev.myplazze.com/api/v1/practice/barrios');
        this.barrios = response.data;
      } catch (error) {
        console.error('Error al obtener los barrios:', error);
      }
    },
    // Obtener la lista de apartamentos desde la API
    async fetchApartments() {
      try {
        const response = await axios.post(
          'https://api.dev.myplazze.com/api/v1/practice/search',
          {
            "barrio_id": null,
            "bathrooms": null,
            "bedrooms": null,
            "guest": null,
            "max_price": null,
            "min_price": null,
          }
        );

        // Almacenar la lista de apartamentos y establecer los apartamentos filtrados
        this.apartments = response.data;
        this.filteredApartments = this.apartments;
        console.log(response.data)
      } catch (error) {
        console.error('Error al obtener los apartamentos:', error);
      }
    },
    // Filtrar la lista de apartamentos según el barrio seleccionado
    filterApartments() {
      if (this.selectedBarrio === 'todos') {
        this.filteredApartments = this.apartments;
      } else {
        this.filteredApartments = this.apartments.filter((apartment) => {
          return (
            apartment.barrio.name === this.selectedBarrio &&
            (!this.numRooms || apartment.bedrooms === parseInt(this.numRooms)) &&
            (!this.numBath || apartment.bathrooms === parseInt(this.numBath)) &&
            (!this.maxAccommodates || apartment.accommodates_max <= parseInt(this.maxAccommodates))
          );
        });
      }
    },
    // Selección de barrio
    handleBarrioSelected(barrio) {
      this.selectedBarrio = barrio;
      this.filterApartments();
    },
    // Abrir el modal de filtros
    openFilterModal() {
      this.$refs.filterModal.openModal();
    },
    // Manejar la aplicación de filtros y obtener apartamentos filtrados
    handleFiltersApplied(filters) {
      this.filterApartments();
      this.fetchFilteredApartments(filters);
    },
    // Obtener apartamentos filtrados desde la API
    async fetchFilteredApartments(filters) {
      try {
        const response = await axios.post(
          'https://api.dev.myplazze.com/api/v1/practice/search',
          {
            "barrio_id": this.selectedBarrio === 'todos' ? null : this.selectedBarrio,
            "bedrooms": filters.numRooms,
            "bathrooms": filters.numBath,
            "accommodates_max": filters.maxAccommodates,
            "min_price": filters.minPrice,
            "max_price": filters.maxPrice,
          }
        );

        this.filteredApartments = response.data;
      } catch (error) {
        console.error('Error al obtener los apartamentos filtrados:', error);
      }
    },
    // Manejar el reinicio de filtros y obtener la lista completa de apartamentos
    handleFiltersReset() {
      this.fetchApartments();
    }
  },
};
</script>
