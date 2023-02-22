<script setup>
import store from './../store'
</script>

<template>
  <br/>
  <h2>TheWelcome.vue</h2>
    Clickable Token from TheWelcome.vue: 
    <a @click="store.increment" href="https://vuejs.org/" target="_blank" rel="noopener">
      {{ store.token }}</a>
</template>
