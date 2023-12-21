<template>
  <div class="py-4 px-10 flex justify-between items-center">
    <BarrioSelect :barrios="barriosData" v-model="selectedBarrio" />
    <FilterModal v-model="filterModalVisible" @apply-filters="applyFilters" />
    <SearchButton @click="performSearch" />
  </div>
</template>

<script>
import BarrioSelect from "@/components/Filtros/BarrioSelect.vue";
import FilterModal from "@/components/Filtros/FilterModal.vue";
import SearchButton from "@/components/Filtros/SearchButton.vue";
import axios from "axios";

export default {
  components: {
    BarrioSelect,
    FilterModal,
    SearchButton,
  },
  props: {
    barrios: {
      type: Array,
      required: true,
    },
  },
  data() {
    return {
      selectedBarrio: null,
      filterModalVisible: false,
      priceRange: { min: null, max: null },
      bedrooms: null,
      guests: null,
      barriosData: [], // Initialize barriosData as an empty array
    };
  },
  computed: {
    filteredApartments() {
      // Filter the apartments based on the selected barrio
      return this.apartments.filter(apartment => {
        return !this.selectedBarrio || apartment.barrio.name === this.selectedBarrio;
      });
    },
  },
  mounted() {
    // Fetch barrios data when the component is mounted
    this.fetchBarrios();
  },
  methods: {
    async fetchBarrios() {
      try {
        const response = await axios.get("https://api.dev.myplazze.com/api/v1/practice/barrios");
        // Assuming the response contains an array of barrios
        this.barriosData = response.data; // Update barriosData
        this.$emit("update-barrios", this.barriosData);
        console.log(response.data);
      } catch (error) {
        console.error("Error fetching barrios:", error);
      }
    },
    performSearch() {
      // ... your existing search logic
    },
    applyFilters(filters) {
      // ... your existing filter logic
    },
    // Define a function to be passed to ApartmentCard.vue
    handleClickOnApartment(apartment) {
      // Your logic when an apartment card is clicked
      console.log("Apartment clicked:", apartment);
    },
  },
};
</script>
