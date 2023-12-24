<template>
  <!-- Contenedor principal de la tarjeta de apartamento -->
  <div class="bg-white shadow-lg p-4 w-64 md:w-72 lg:w-96 xl:w-1/4 m-4 rounded-2xl">
    <!-- Contenedor del carrusel de imágenes -->
    <div
      class="swiper-container overflow-hidden rounded-lg h-20 md:h-56 lg:h-64"
      @mouseenter="handleMouseEnter"
      @mouseleave="handleMouseLeave"
    >
      <!-- Contenido del carrusel -->
      <div class="swiper-wrapper cursor-grab">
        <!-- Iteración sobre las fotos del apartamento -->
        <div class="swiper-slide" v-for="(photo, index) in apartment.pic" :key="index">
          <!-- Imagen del apartamento -->
          <img :src="photo" :alt="apartment.apartment_title" class="object-cover w-full h-full" />
        </div>
      </div>
      
      <!-- Paginación del carrusel -->
      <div class="swiper-pagination"></div>
    </div>
    
    <!-- Información del apartamento con animación al pasar el mouse -->
    <div
      class="apartment-info text-gray-800"
      :class="{ 'lift-up': hovered }"
      @mouseover="hovered = true"
      @mouseleave="hovered = false"
    >
      <!-- Contenedor de la información desplegada -->
      <div class="displayed-info flex flex-col gap-1 bg-white rounded-3xl p-4">
        <!-- Título, dirección y precio del apartamento -->
        <div class="flex flex-col gap-2">
          <h2 class="text-xl lg:text-2xl xl:text-3xl font-bold">{{ apartment.apartment_title }}</h2>
          <p class="text-gray-600 text-sm lg:text-base xl:text-lg">{{ apartment.address }}</p>
          <p class="text-green-500 font-bold text-lg lg:text-xl xl:text-2xl">{{ formatCurrency(apartment.monthly_price) }}</p>
        </div>

        <!-- Información oculta sobre habitaciones, baños, metros cuadrados y comodidades -->
        <div class="flex flex-row justify-between text-sm lg:text-base xl:text-lg">
          <div class="hidden-info mt-5">
            <p>{{ apartment.bedrooms }} habitaciones</p>
            <p>{{ apartment.bathrooms }} baños</p>
            <p>{{ apartment.square_meter }} m²</p>
          </div>
          <div class="hidden-info mt-5">
            <p>Wifi: {{ apartment.amenities.wifi ? 'Si' : 'No' }}</p>
            <p>A/C: {{ apartment.amenities['A/C'] ? 'Si' : 'No' }}</p>
            <p>Calefaccion: {{ apartment.amenities.heating ? 'Si' : 'No' }}</p>
          </div>
        </div>
      </div>
    </div>
    
    <!-- Botón para regresar arriba y mostrarlo cuando se desplaza hacia abajo -->
    <button @click="goToTop" class="go-top-btn" v-show="showGoTopButton"><span class="icon-btn">👆</span></button>
  </div>
</template>

<script>
import Swiper from 'swiper';
import 'swiper/swiper-bundle.css';

export default {
  // Propiedades recibidas desde el componente padre
  props: {
    apartment: Object,
  },
  // Datos del componente de la tarjeta de apartamento
  data() {
    return {
      hovered: false,
      isMouseDown: false,
      swiper: null,
      showGoTopButton: false,
    };
  },
  // Método ejecutado después de que la instancia es creada
  mounted() {
    // Agregar evento de desplazamiento para mostrar el botón "Regresar arriba"
    window.addEventListener('scroll', this.handleScroll);
    
    // Inicializar el carrusel de imágenes con Swiper
    this.swiper = new Swiper('.swiper-container', {
      slidesPerView: 1,
      pagination: {
        el: '.swiper-pagination',
        clickable: true,
      },
    });
  },
  // Método ejecutado antes de destruir la instancia del componente
  beforeDestroy() {
    // Eliminar el evento de desplazamiento antes de destruir el componente
    window.removeEventListener('scroll', this.handleScroll);
  },
  // Métodos del componente
  methods: {
    // Formatear el valor de la moneda
    formatCurrency(value) {
      return new Intl.NumberFormat('es-ES', { style: 'currency', currency: 'EUR' }).format(value);
    },
    // Manejar el evento de desplazamiento
    handleScroll() {
      this.showGoTopButton = window.scrollY > 200;
    },
    // Desplazarse hacia arriba suavemente
    goToTop() {
      window.scrollTo({
        top: 0,
        behavior: 'smooth',
      });
    },
    // Manejar el evento al presionar el mouse
    handleMouseDown() {
      document.body.style.cursor = 'grabbing';
      this.isMouseDown = true;
    },
    // Manejar el evento al soltar el mouse
    handleMouseUp() {
      document.body.style.cursor = this.hovered ? 'grab' : 'auto';
      this.isMouseDown = false;
    },
  },
};
</script>

  
<style lang="scss" scoped>

  .go-top-btn {
    position: fixed;
    bottom: 20px;
    right: 20px;
    padding: 10px;
    background-color: #153e68;
    color: #fff;
    border: none;
    border-radius: 5px;
    cursor: pointer;
    z-index: 9999;

    &:hover {
      span.icon-btn {
        display: inline-block;
        animation: bounce 0.5s ease infinite;
      }
    }
  }
  @keyframes bounce {
    0%, 20%, 50%, 80%, 100% {
      transform: translateY(0);
    }
    40% {
      transform: translateY(-10px);
    }
    60% {
      transform: translateY(-5px);
    }
  }

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
  