<template>
  <v-card class="pa-8" elevation="2">
    <!-- Campo de búsqueda por nombre -->
    <v-text-field
      v-model="nombre"
      label="Buscar autor por nombre"
      prepend-icon="mdi-account-search"
      clearable
    ></v-text-field>

    <v-btn color="primary" class="mt-2" @click="buscarAutores">
      Buscar
    </v-btn>

    <!-- Mensaje de error -->
    <v-alert
      v-if="error"
      type="error"
      class="mt-4"
    >
      {{ error }}
    </v-alert>

    <!-- Resultados de autores -->
    <v-container fluid class="mt-4">
      <v-row dense>
        <v-col
          v-for="autor in autores"
          :key="autor.autorLibroGuid"
          cols="12"
          md="4"
        >
          <v-card class="mb-4" border flat>
            <v-card-title class="text-h6">
              {{ autor.nombre }} {{ autor.apellido }}
            </v-card-title>

            <v-card-text>
              <p><strong>ID:</strong> {{ autor.autorLibroId }}</p>
              <p><strong>Fecha de Nacimiento:</strong> {{ autor.fechaFormateada }}</p>
              <p><strong>GUID:</strong> {{ autor.autorLibroGuid }}</p>
            </v-card-text>
          </v-card>
        </v-col>
      </v-row>

      <v-alert
        v-if="!autores.length && !error && buscarPresionado"
        type="info"
        class="mt-4"
      >
        No se encontraron autores con ese nombre.
      </v-alert>
    </v-container>
  </v-card>
</template>

<script setup>
import { ref } from 'vue'

const nombre = ref('')
const autores = ref([])
const error = ref(null)
const buscarPresionado = ref(false)

const formatearFecha = (isoDate) => {
  const date = new Date(isoDate)
  return date.toLocaleDateString('es-MX', {
    day: '2-digit',
    month: '2-digit',
    year: 'numeric'
  })
}

const buscarAutores = async () => {
  autores.value = []
  error.value = null
  buscarPresionado.value = true

  if (!nombre.value.trim()) {
    error.value = 'Debes ingresar un nombre.'
    return
  }

  try {
    const url = `http://autores.somee.com/api/Autor/nombre?nombre=${encodeURIComponent(nombre.value)}`
    const response = await fetch(url)

    if (!response.ok) throw new Error('Error al buscar autores')

    const data = await response.json()
    const lista = Array.isArray(data) ? data : [data]

    autores.value = lista.map(autor => ({
      ...autor,
      fechaFormateada: formatearFecha(autor.fechaNacimiento)
    }))
  } catch (err) {
    error.value = err.message
  }
}

</script>
