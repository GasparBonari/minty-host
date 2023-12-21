<template>
  <div id="app">
    <NavbarResponsive />
    <Filtro />
    <div class="flex flex-wrap justify-center py-10">
      <ApartmentCard
        v-for="apartment in filteredApartments"
        :key="apartment.id"
        :apartment="apartment"
        :onClick="handleClickOnApartment"
      />
    </div>
  </div>
</template>

<script>
import NavbarResponsive from "@/components/NavbarResponsive.vue";
import ApartmentCard from "@/components/Apartamentos/ApartmentCard.vue";
import Filtro from "@/components/Filtro.vue";
import axios from "axios";

export default {
  name: "App",
  components: {
    NavbarResponsive,
    Filtro,
    ApartmentCard,
  },
  data() {
    return {
      apartments: [],
      filteredApartments: [],
    };
  },
  mounted() {
    // Llamada al endpoint para obtener la lista de apartamentos
    this.fetchApartments();
  },
  methods: {
    async fetchApartments() {
      try {
        const response = await axios.post(
          "https://api.dev.myplazze.com/api/v1/practice/search",
          {
            "barrio_id": null,
            "bedrooms": null,
            "guests": null,
            "min_price": null,
            "max_price": null,
          }
        );

        this.apartments = response.data; // Asigna la respuesta a la lista de apartamentos
        console.log(response.data);
        this.filteredApartments = this.apartments; // Inicialmente, muestra todos los apartamentos
      } catch (error) {
        console.error("Error fetching apartments:", error);
      }
    },
  },
};
</script>

<style>
/* Add any global styles or overrides here */
</style>
