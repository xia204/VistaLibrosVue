<template>
  <v-data-table
    :headers="headers"
    :items="libros"
    :loading="loading"
    class="elevation-1"
    items-per-page="5"
  >
    <template #no-data>
      No se encontraron libros.
    </template>
  </v-data-table>
</template>

<script setup>
import { ref, onMounted } from 'vue'

const libros = ref([])
const loading = ref(false)

const headers = [
  { title: 'ID', value: 'libreriaMateriaId' },
  { title: 'Título', value: 'titulo' },
  { title: 'Fecha de Publicación', value: 'fechaFormateada' },
  { title: 'Autor (ID)', value: 'autorLibro' },
]

const formatearFecha = (isoDate) => {
  const date = new Date(isoDate)
  return date.toLocaleDateString('es-MX', {
    day: '2-digit',
    month: '2-digit',
    year: 'numeric'
  })
}

const cargarLibros = async () => {
  loading.value = true
  try {
    const response = await fetch('https://localhost:7065/api/LibroMaterial')
    if (!response.ok) throw new Error('Error al obtener datos')
    const data = await response.json()

    // Agrega campo formateado
    libros.value = data.map(libro => ({
      ...libro,
      fechaFormateada: formatearFecha(libro.fechaPublicacion)
    }))
  } catch (error) {
    console.error('Error al cargar libros:', error)
  } finally {
    loading.value = false
  }
}

onMounted(cargarLibros)
</script>
