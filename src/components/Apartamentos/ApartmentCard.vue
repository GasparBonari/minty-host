<template>
    <div class="bg-white shadow-lg p-4 w-64 md:w-72 lg:w-96 xl:w-1/4 m-4 rounded-2xl">
        <div
        class="swiper-container overflow-hidden rounded-lg h-20 md:h-56 lg:h-64"
        @mouseenter="handleMouseEnter"
        @mouseleave="handleMouseLeave"
        >
        <div class="swiper-wrapper cursor-grab">
            <div
                class="swiper-slide"
                v-for="(photo, index) in apartment.pic"
                :key="index"
            >
                    <img :src="photo" :alt="apartment.apartment_title" class="object-cover w-full h-full" />
                </div>
            </div>
            <!-- Add pagination -->
            <div class="swiper-pagination"></div>
        </div>
        <div
            class="apartment-info text-gray-800"
            :class="{ 'lift-up': hovered }"
            @mouseover="hovered = true"
            @mouseleave="hovered = false"
        >
            <div class=" displayed-info bg-white rounded-3xl p-4">
                <div class="flex flex-col gap-2">
                    <h2 class="text-xl font-bold">{{ apartment.apartment_title }}</h2>
                    <p class="text-gray-600">{{ apartment.address }}</p>
                    <p class="text-green-500 font-bold">{{ formatCurrency(apartment.monthly_price) }}</p>
                </div>
                <!-- Otros detalles del apartamento -->
                <div class="hidden-info mt-5">
                    <p>{{ apartment.barrio.name }} barrio</p>
                    <p>{{ apartment.bedrooms }} habitaciones</p>
                    <p>{{ apartment.bathrooms }} baños</p>
                    <p>{{ apartment.square_meter }} m²</p>
                <!-- Otros detalles -->
                </div>
            </div>
        </div>
    </div>

  </template>
  
  <script>
  import Swiper from 'swiper';
  import 'swiper/swiper-bundle.css';

  
  export default {
    props: {
      apartment: Object, // Modelo de datos del apartamento
    },
    data() {
      return {
        hovered: false,
        isMouseDown: false,
        swiper: null,
      };
    },
    mounted() {
      // Initialize Swiper after component is mounted
      this.swiper = new Swiper('.swiper-container', {
        slidesPerView: 1,
        pagination: {
          el: '.swiper-pagination',
          clickable: true,
        },
      });
    },
    methods: {
        formatCurrency(value) {
            return new Intl.NumberFormat('es-ES', { style: 'currency', currency: 'EUR' }).format(value);
        },

        handleMouseDown() {
            document.body.style.cursor = 'grabbing';
            this.isMouseDown = true;
        },

        handleMouseUp() {
            document.body.style.cursor = this.hovered ? 'grab' : 'auto';
            this.isMouseDown = false;
        },
    },
  };
  </script>
  
  <style lang="scss" scoped>
  .lift-up {
    .displayed-info {
      transform: translateY(-15%);
    }

    .hidden-info {
      opacity: 1;
      transform: translateY(0);
      transition: opacity 0.3s ease-in, transform 0.3s ease-in;
    }
  }

  .hidden-info {
    opacity: 0;
    transform: translateY(100%);
    transition: opacity 0.3s ease-out, transform 0.3s ease-out;
  }

  .apartment-info {
    z-index: 2;
    position: relative;
    background-color: white;
  }
</style>
  