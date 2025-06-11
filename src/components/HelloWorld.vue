<script setup>
import { ref, shallowRef } from 'vue'
import VistaConsulta from '@/views/VistaConsulta.vue'
import LibroPost from '@/components/LibroPost.vue'

const drawer = ref(true)

const links = [
  ['mdi-database', 'Consulta', VistaConsulta],
  ['mdi-send', 'Crear', LibroPost]
]

const componenteActual = shallowRef(VistaConsulta)
</script>

<template>
  <v-app id="inspire">
    <v-navigation-drawer v-model="drawer">
      <v-sheet class="pa-4" color="grey-lighten-4">
        <v-avatar class="mb-4" size="64">
          <v-img alt="Avatar" src="../assets/avatar.jpeg"></v-img>
        </v-avatar>
        <div>xia@google.com</div>
      </v-sheet>

      <v-divider></v-divider>

      <v-list>
        <v-list-item
          v-for="[icon, text, value] in links"
          :key="icon"
          :prepend-icon="icon"
          :title="text"
          link
          @click="componenteActual = value"
        ></v-list-item>
      </v-list>
    </v-navigation-drawer>

    <v-main>
      <v-container class="py-8 px-6" fluid>
        <component :is="componenteActual" />
      </v-container>
    </v-main>
  </v-app>
</template>
