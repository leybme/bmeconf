<!-- src/App.vue -->
<script setup>
import { ref } from 'vue'

import HelloWorld from './components/HelloWorld.vue'
import TheWelcome from './components/TheWelcome.vue'
import GoogleSheet from './components/GoogleSheet.vue';
import MainToolbar from './components/MainToolbar.vue';
const theme = ref("light")
function refreshGoogleSheet() {
  // Add your refresh logic here
  console.log('Refreshing GoogleSheet component');
}
</script>

<template >
  <v-responsive >
    <v-app :theme="theme">
      <MainToolbar :theme="theme" @toggle-theme="theme = $event" @refresh="refreshGoogleSheet" />
      <v-main :theme="theme">
        <v-container :theme="theme">
          <GoogleSheet />
        </v-container>
      </v-main>
    </v-app>
  </v-responsive>
  
</template>


<style scoped>

</style>
