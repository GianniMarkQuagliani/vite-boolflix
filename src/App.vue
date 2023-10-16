<script>
import axios from 'axios';
import { store } from './data/store';
export default {
  name: 'App',
  data() {
    return {
      store,
    };
  },
  methods: {
    apiKey() {
      // axios get api key
      console.log(store.apiKey);
      axios.get(store.apiKey)
      .then((res) => {
        console.log(res.data);
      })
      // error
      .catch((err) => {
        console.log(err);
      })
    }
  },
  mounted() {
    this.apiKey();
  }
}
</script>


<template>
  <h1>Ciao</h1>
</template>


<style lang="scss" scoped>
@use './scss/main.scss' as *;
</style>