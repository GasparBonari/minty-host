<template>
    <p>filter modal here</p>
  </template>
  
  <script>
  export default {
    data() {
      return {
        // Add data properties for modal filters
      };
    },
    methods: {
      applyFilters() {
        // Collect the selected filters and emit an event to parent (Filtro.vue)
        const filters = {
          priceRange: { min: this.minPrice, max: this.maxPrice },
          bedrooms: this.selectedBedrooms,
          guests: this.selectedGuests,
          // ... other filters
        };
        this.$emit("apply-filters", filters);
      },
    },
  };
  </script>