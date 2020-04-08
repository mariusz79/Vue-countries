<template>
  <div class="home">
    <img alt="Vue logo" src="../assets/logo.png">
    <HelloWorld msg="Welcome to Your Vue.js App"/>
    <button @click="get()">Get</button>
  </div>
</template>

<script>
// @ is an alias to /src
import HelloWorld from '@/components/HelloWorld.vue';
import axios from 'axios';

export default {
  name: 'Home',
  components: {
    HelloWorld,
  },
  methods: {
    get() {
      axios.get('https://restcountries.eu/rest/v2/alpha/col')
        .then((response) => {
          console.log(response);
        });
    },
  },
};
</script>
