<script setup>
import HelloWorld from './components/HelloWorld.vue'
import TheWelcome from './components/TheWelcome.vue'
import GoogleSheet from './components/GoogleSheet.vue';
import MainToolbar from './components/MainToolbar.vue';
</script>

<template>
  <!-- <header>
    <img alt="Vue logo" class="logo" src="./assets/logo.svg" width="125" height="125" />
    <div class="wrapper">
      <HelloWorld msg="You did it!" />
    </div>
  </header> -->
<div>
    <MainToolbar @refresh="refreshGoogleSheet" />
</div>
<div>
   <GoogleSheet />
</div>
  
  <!-- <main> -->
    <!-- <TheWelcome /> -->
    <!-- <GoogleSheet />
  </main> -->
</template>

<style scoped>
/* header {
  line-height: 1.5;
} */

/* .logo {
  display: block;
  margin: 0 auto 2rem;
} */
main {
  padding: 20px;
  padding-top: 60px; /* Adjust the padding-top to make space for the fixed toolbar */
}

/* @media (min-width: 1024px) {
  header {
    display: flex;
    place-items: center;
    padding-right: calc(var(--section-gap) / 2);
  }

  .logo {
    margin: 0 2rem 0 0;
  }

  header .wrapper {
    display: flex;
    place-items: flex-start;
    flex-wrap: wrap;
  }
} */
</style>
