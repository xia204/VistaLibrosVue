<template>
  <div>
    <!--<TableLibros />-->
    <IteratorLibros />
    
    <v-divider class="my-8"></v-divider>
    <h2 class="text-h5">Consulta de Libro por ID</h2>
    <LibroPorId class="mt-8" />
  </div>
</template>

<script setup>
//import TableLibros from '../components/TableLibros.vue'
import LibroPorId from '../components/LibroId.vue'
import IteratorLibros from '@/components/IteratorLibros.vue';
</script>
