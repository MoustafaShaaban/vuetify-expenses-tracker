<script setup>
import { ref } from 'vue';
import { RouterView } from 'vue-router';
import { useTheme } from 'vuetify'

const theme = useTheme();

function toggleTheme() {
  theme.global.name.value = theme.global.current.value.dark ? 'light' : 'dark'
}

const drawer = ref();
</script>

<template>
  <v-layout>
    <v-app-bar>
      <template v-slot:prepend>
        <v-app-bar-nav-icon variant="text" @click.stop="drawer = !drawer"></v-app-bar-nav-icon>

      </template>
      <v-app-bar-title>
        <v-btn to="/">Home</v-btn>
      </v-app-bar-title>
    </v-app-bar>

    <v-navigation-drawer v-model="drawer">
      <v-list>
        <v-list-item prepend-icon="mdi-view-dashboard" title="Home" to="/"></v-list-item>
        <v-list-item prepend-icon="mdi-account-box" title="Account"></v-list-item>
        <v-list-item prepend-icon="mdi-gavel" title="About" to="/about"></v-list-item>
        <v-btn @click="toggleTheme">toggle theme</v-btn>
      </v-list>

      <!-- <template v-slot:append>
        <div class="pa-2">
          <v-btn block>
            Logout
          </v-btn>
        </div>
      </template> -->
    </v-navigation-drawer>

    <v-main>
      <RouterView />
    </v-main>
  </v-layout>
</template>