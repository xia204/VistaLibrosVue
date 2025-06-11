<template>
  <v-card class="pa-4" elevation="2">
    <v-text-field v-model="titulo" label="Título" />
    <v-text-field
      v-model="fechaPublicacion"
      label="Fecha de Publicación"
      type="datetime-local"   
    />
    <v-text-field v-model="autorLibro" label="Autor ID" />

    <v-btn class="mt-4" color="primary" @click="crearLibro">Crear</v-btn>

    <v-alert v-if="mensaje" class="mt-4" type="success">
      {{ mensaje }}
    </v-alert>
  </v-card>
</template>

<script setup>
import { ref } from 'vue'

// Variables del formulario
const titulo = ref('')
const fechaPublicacion = ref('')
const autorLibro = ref('')
const mensaje = ref('')

// Función para enviar el POST
const crearLibro = async () => {
  try {
    const body = {
      titulo: titulo.value,
      fechaPublicacion: new Date(fechaPublicacion.value).toISOString(), // Formato ISO
      autorLibro: autorLibro.value,
    }

    const response = await fetch('https://localhost:7065/api/LibroMaterial', {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
      },
      body: JSON.stringify(body),
    })

    if (!response.ok) throw new Error('Error al crear el libro')

    mensaje.value = 'Libro creado correctamente ✅'
    // Limpiar campos
    titulo.value = ''
    fechaPublicacion.value = ''
    autorLibro.value = ''
  } catch (error) {
    mensaje.value = `❌ ${error.message}`
  }
}
</script>
