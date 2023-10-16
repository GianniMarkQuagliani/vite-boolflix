<script>
import axios from 'axios';
import { store } from './data/store';
import Header from './components/Partials/Header.vue';
import Main from './components/Partials/Main.vue';
export default {
  name: 'App',
  components: {
    Header,
    Main,
  },
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
  <Header />
  <Main />
</template>


<style lang="scss" scoped>
@use './scss/main.scss' as *;
</style>