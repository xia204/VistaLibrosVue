<template>
  <v-card class="pa-4" elevation="2">
    <v-text-field v-model="nombre" label="Nombre" />
    <v-text-field v-model="apellido" label="Apellido" />
    <v-text-field
      v-model="fechaNacimiento"
      label="Fecha de Nacimiento"
      type="date"
    />

    <v-btn class="mt-4" color="primary" @click="crearAutor">Crear Autor</v-btn>

    <v-alert v-if="mensaje" class="mt-4" :type="esError ? 'error' : 'success'">
      {{ mensaje }}
    </v-alert>
  </v-card>
</template>

<script setup>
import { ref } from 'vue'

// Variables del formulario
const nombre = ref('')
const apellido = ref('')
const fechaNacimiento = ref('')
const mensaje = ref('')
const esError = ref(false)

// Función para enviar el POST
const crearAutor = async () => {
  try {
    const body = {
      nombre: nombre.value,
      apellido: apellido.value,
      fechaNacimiento: new Date(fechaNacimiento.value).toISOString(),
    }

    const response = await fetch('http://autores.somee.com/api/Autor', {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
      },
      body: JSON.stringify(body),
    })

    if (!response.ok) throw new Error('Error al crear el autor')

    mensaje.value = 'Autor creado correctamente ✅'
    esError.value = false

    // Limpiar campos
    nombre.value = ''
    apellido.value = ''
    fechaNacimiento.value = ''
  } catch (error) {
    mensaje.value = `❌ ${error.message}`
    esError.value = true
  }
}
</script>
