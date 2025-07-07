<template>
  <v-card class="pa-4" elevation="2">
    <v-data-iterator
      :items="autores"
      :items-per-page="6"
      :loading="loading"
    >
      <template v-slot:header>
        <v-toolbar class="px-2">
          <v-toolbar-title>Lista de Autores</v-toolbar-title>
        </v-toolbar>
      </template>

      <template v-slot:default="{ items }">
        <v-container fluid>
          <v-row dense>
            <v-col
              v-for="item in items"
              :key="item.raw.autorLibroGuid"
              cols="12"
              md="4"
            >
              <v-card class="mb-4" border flat>
                <v-img
                  :src="item.raw.imagenUrl"
                  height="200"
                  cover
                ></v-img>

                <v-card-title class="text-h6">
                  {{ item.raw.nombre }} {{ item.raw.apellido }}
                </v-card-title>

                <v-card-text>
                  <p><strong>ID:</strong> {{ item.raw.autorLibroId }}</p>
                  <p><strong>Fecha de Nacimiento:</strong> {{ item.raw.fechaFormateada }}</p>
                  <p><strong>GUID:</strong> {{ item.raw.autorLibroGuid }}</p>
                </v-card-text>
              </v-card>
            </v-col>
          </v-row>
        </v-container>

        <v-alert v-if="!items.length && !loading" type="info" class="mt-4">
          No se encontraron autores.
        </v-alert>
      </template>

      <template v-slot:footer="{ page, pageCount, prevPage, nextPage }">
        <div class="d-flex align-center justify-center pa-4">
          <v-btn
            :disabled="page === 1"
            icon="mdi-arrow-left"
            variant="tonal"
            @click="prevPage"
          ></v-btn>

          <div class="mx-2 text-caption">Página {{ page }} de {{ pageCount }}</div>

          <v-btn
            :disabled="page >= pageCount"
            icon="mdi-arrow-right"
            variant="tonal"
            @click="nextPage"
          ></v-btn>
        </div>
      </template>
    </v-data-iterator>
  </v-card>
</template>

<script setup>
import { ref, onMounted } from 'vue'
import authorImage from '@/assets/avatar.jpeg'

const autores = ref([])
const loading = ref(false)

const formatearFecha = (isoDate) => {
  const date = new Date(isoDate)
  return date.toLocaleDateString('es-MX', {
    day: '2-digit',
    month: '2-digit',
    year: 'numeric'
  })
}

const cargarAutores = async () => {
  loading.value = true
  try {
    const response = await fetch('http://autores.somee.com/api/Autor')
    if (!response.ok) throw new Error('Error al obtener datos')
    const data = await response.json()

    autores.value = data.map(autor => ({
      ...autor,
      fechaFormateada: formatearFecha(autor.fechaNacimiento),
      imagenUrl: authorImage
    }))
  } catch (error) {
    console.error('Error al cargar autores:', error)
  } finally {
    loading.value = false
  }
}

onMounted(cargarAutores)
</script>
