<template>
    <nav class="bg-gray-800 p-4 flex justify-between items-center">
      <!-- Logo Minty Host -->
      <img src="https://www.mintyhost.com/wp-content/uploads/2023/06/mintyhost-logotipo.svg" alt="Minty Host Logo" class="w-21 h-10 cursor-pointer">
  
      <!-- Hamburger para Mobile -->
      <button @click="toggleMobileNav" class="md:hidden text-white focus:outline-none">
        <svg class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg">
          <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 6h16M4 12h16m-7 6h7"></path>
        </svg>
      </button>
  
      <!-- Listado de Páginas en Desktop -->
      <div class="hidden md:flex space-x-4">
        <router-link to="/" class="text-white cursor-pointer">Inicio</router-link>
        <router-link to="/viviendas" class="text-white cursor-pointer">Viviendas</router-link>
        <router-link to="/contacto" class="text-white cursor-pointer">Contacto</router-link>
        <router-link to="/preguntas" class="text-white cursor-pointer">Preguntas</router-link>
      </div>
  
      <!-- Selector de Idioma en Desktop -->
      <div class="hidden md:block">
        <select v-model="selectedLanguage" class="text-white bg-gray-700 p-2 rounded cursor-pointer">
          <option value="es">Español</option>
          <option value="en">English</option>
        </select>
      </div>
  
      <!-- Navbar Mobile -->
      <div v-if="isMobileNavOpen" class="md:hidden absolute top-16 left-0 right-0 bg-gray-800 p-4 space-y-5 flex flex-col items-center">
        <router-link to="/" class="text-white block cursor-pointer">Inicio</router-link>
        <router-link to="/viviendas" class="text-white block cursor-pointer">Viviendas</router-link>
        <router-link to="/contacto" class="text-white block cursor-pointer">Contacto</router-link>
        <router-link to="/preguntas" class="text-white block cursor-pointer">Preguntas</router-link>
  
        <!-- Selector de Idioma en Mobile -->
        <div class="mt-4">
          <select v-model="selectedLanguage" class="text-white bg-gray-700 p-2 rounded cursor-pointer">
            <option value="es">Español</option>
            <option value="en">English</option>
          </select>
        </div>
      </div>
    </nav>
  </template>
  
  <script>
  export default {
    data() {
      return {
        isMobileNavOpen: false,
        selectedLanguage: "es",
      };
    },
    methods: {
      toggleMobileNav() {
        this.isMobileNavOpen = !this.isMobileNavOpen;
      },
    },
  };
  </script>