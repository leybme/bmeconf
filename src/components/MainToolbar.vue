<!-- scr/components/MainToolbar.vue -->
<template>
  <v-app-bar :theme="theme" :elevation="5">
    <v-toolbar-title>BME10 Conference</v-toolbar-title>
    <template v-slot:prepend>
    <v-app-bar-nav-icon></v-app-bar-nav-icon>
  </template>
    <v-spacer></v-spacer>
    <v-btn :prepend-icon="theme === 'light' ? 'mdi-weather-sunny' : 'mdi-weather-night'" text="Toggle Theme" slim
      @click="onClick"></v-btn>
      <template v-slot:append>
    <v-btn icon="mdi-heart"></v-btn>

    <v-btn icon="mdi-magnify"></v-btn>

    <v-btn icon="mdi-dots-vertical"></v-btn>
  </template>
  </v-app-bar>
</template>

<script setup>
import { ref, watch } from 'vue'

const props = defineProps({
  theme: {
    type: String,
    required: true
  }
})
const theme = ref(props.theme)
function onClick() {
  theme.value = theme.value === 'light' ? 'dark' : 'light'
  emit('toggle-theme', theme.value)
}
// Watch for prop changes to sync theme
watch(() => props.theme, (newTheme) => {
  theme.value = newTheme
})
const emit = defineEmits(['toggle-theme', 'refresh'])
</script>
