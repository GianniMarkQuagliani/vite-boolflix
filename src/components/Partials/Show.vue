<template>
    <div class="row row-cols-auto justify-content-between g-0">
      <ElementCard
        :key="item.id"
        v-for="item in showData"
        :title="item.name"
        :originalTitle="item.original_name"
        :language="item.original_language"
        :voteAverage="String(item.vote_average)"
        :image="item.poster_path"
      />
    </div>
  </template>
  
  <script>
  import ElementCard from "./Card.vue";
  export default {
    name: "Show",
    components: {
      Card,
    },
    props: {
      showData: Array,
    },
  };
  </script>
  