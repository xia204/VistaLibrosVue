<template>
  <v-card class="pa-16" elevation="2">
    <!-- Campo de búsqueda -->
    <v-text-field
      v-model="id"
      label="Ingrese el ID del libro"
      prepend-icon="mdi-magnify"
      type="text"
      clearable
    ></v-text-field>

    <v-btn color="primary" @click="buscarLibro">
      Buscar
    </v-btn>

    <!-- Error -->
    <v-alert
      v-if="error"
      type="error"
      class="mt-4"
    >
      {{ error }}
    </v-alert>

    <!-- Tarjeta del libro encontrado -->
    <v-card
      v-if="libro"
      class="mt-4 mx-auto"
      max-width="400"
      elevation="1"
      border
      flat
    >
      <v-img
        :src="libro.imagenUrl"
        height="280"
        cover
      ></v-img>

      <v-card-title class="text-h6">
        {{ libro.titulo }}
      </v-card-title>

      <v-card-text>
        <p><strong>ID:</strong> {{ libro.libreriaMateriaId }}</p>
        <p><strong>Fecha de Publicación:</strong> {{ libro.fechaFormateada }}</p>
        <p><strong>Autor (ID):</strong> {{ libro.autorLibro }}</p>
      </v-card-text>
    </v-card>
  </v-card>
</template>

<script setup>
import { ref } from 'vue'
import bookImage from '@/assets/book.jpg' // ← Importación de la imagen

const id = ref('')
const libro = ref(null)
const error = ref(null)

const formatearFecha = (isoDate) => {
  const date = new Date(isoDate)
  return date.toLocaleDateString('es-MX', {
    day: '2-digit',
    month: '2-digit',
    year: 'numeric'
  })
}

const buscarLibro = async () => {
  libro.value = null
  error.value = null

  if (!id.value.trim()) {
    error.value = 'Debes ingresar un ID válido.'
    return
  }

  try {
    const response = await fetch(`https://localhost:7065/api/LibroMaterial/${id.value}`)
    if (!response.ok) throw new Error('Libro no encontrado')
    const data = await response.json()

    libro.value = {
      ...data,
      fechaFormateada: formatearFecha(data.fechaPublicacion),
      imagenUrl: bookImage
    }
  } catch (err) {
    error.value = err.message
  }
}
</script>
