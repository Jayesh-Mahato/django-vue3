<script setup>
import {ref} from 'vue'
import ApiGetRequest from './components/ApiGetRequest.vue';
import TheWelcome from "./components/TheWelcome.vue";
import store from './store'
const props = defineProps({
  token: String,
  user: String,
});
if (props.token) {
  store.setToken(props.token)
}
const username = props.user ? props.user : "";

let counteg = ref(0)
function incrementeg(event) {
  console.log(event)
  counteg.value++
}
</script>

<template>
  <header>
    <ApiGetRequest />
    {{ store.count }} - {{ store.token }} - {{ username }}
  </header>

  <main>
    <TheWelcome />
  </main>
</template>

<style scoped>
header {
  line-height: 1.5;
}

.logo {
  display: block;
  margin: 0 auto 2rem;
}

@media (min-width: 1024px) {
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
}
</style>
