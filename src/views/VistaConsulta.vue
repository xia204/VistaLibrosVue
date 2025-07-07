<template>
  <div>
    <!--<TableLibros />-->
    <!--<IteratorLibros />-->
    <IteratorAutor/>
    
    <!--<v-divider class="my-8"></v-divider>-->
    <!--<h2 class="text-h5">Consulta de Libro por ID</h2>-->
    <!--<LibroPorId class="mt-8" />-->
    <v-divider class="my-8"></v-divider>
    <h2 class="text-h5">Consulta Autor por ID</h2>
    <AutorId class="mt-8" />
    <v-divider class="my-8"></v-divider>
    <h2 class="text-h5">Consulta Autor por Nombre</h2>
    <AutorNombre class="mt-8" />
    
  </div>
</template>

<script setup>
//import TableLibros from '../components/TableLibros.vue'
//import LibroPorId from '../components/LibroId.vue'
//import IteratorLibros from '@/components/IteratorLibros.vue';

import AutorId from '@/components/AutorId.vue';
import AutorNombre from '@/components/AutorNombre.vue';
import IteratorAutor from '@/components/IteratorAutor.vue';
</script>
