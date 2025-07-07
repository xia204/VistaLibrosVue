<template>
  <v-card class="pa-16" elevation="2">
    <!-- Campo de búsqueda -->
    <v-text-field
      v-model="id"
      label="Ingrese el ID del autor"
      prepend-icon="mdi-magnify"
      type="text"
      clearable
    ></v-text-field>

    <v-btn color="primary" @click="buscarAutor">
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

    <!-- Tarjeta del autor encontrado -->
    <v-card
      v-if="autor"
      class="mt-4 mx-auto"
      max-width="400"
      elevation="1"
      border
      flat
    >
      <v-card-title class="text-h6">
        {{ autor.nombre }} {{ autor.apellido }}
      </v-card-title>

      <v-card-text>
        <p><strong>ID:</strong> {{ autor.autorLibroId }}</p>
        <p><strong>Fecha de Nacimiento:</strong> {{ autor.fechaFormateada }}</p>
        <p><strong>GUID:</strong> {{ autor.autorLibroGuid }}</p>
      </v-card-text>
    </v-card>
  </v-card>
</template>

<script setup>
import { ref } from 'vue'

const id = ref('')
const autor = ref(null)
const error = ref(null)

const formatearFecha = (isoDate) => {
  const date = new Date(isoDate)
  return date.toLocaleDateString('es-MX', {
    day: '2-digit',
    month: '2-digit',
    year: 'numeric'
  })
}

const buscarAutor = async () => {
  autor.value = null
  error.value = null

  if (!id.value.trim()) {
    error.value = 'Debes ingresar un ID válido.'
    return
  }

  try {
    const response = await fetch(`http://autores.somee.com/api/Autor/${id.value}`)
    if (!response.ok) throw new Error('Autor no encontrado')
    const data = await response.json()

    autor.value = {
      ...data,
      fechaFormateada: formatearFecha(data.fechaNacimiento)
    }
  } catch (err) {
    error.value = err.message
  }
}
</script>