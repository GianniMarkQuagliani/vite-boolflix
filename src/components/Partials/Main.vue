<template>
	<!-- Contenitore della griglia di carte -->
	<div class="row row-cols-auto justify-content-between g-0">
	  <!-- Utilizza il componente 'Card' per ogni elemento in 'filmData' -->
	  <!-- Imposta una chiave unica basata sull'ID dell'elemento -->
	  <!-- Itera su ogni elemento in 'filmData' -->
	  <!-- Passa il titolo dell'elemento come proprietà 'title' a 'Card' -->
	  <!-- Passa il titolo originale come proprietà 'originalTitle' -->
	  <!-- Passa la lingua originale come proprietà 'language' -->
	  <!-- Arrotonda la media dei voti a 2 decimali e passa come 'voteAverage' -->
	  <!-- Passa il percorso dell'immagine come proprietà 'image' -->
	  <!-- Passa la trama come proprietà 'overview' -->
	  <!-- Passa l'ID come proprietà 'id' -->
	  <Card
		:key="item.id"
		v-for="item in filmData"
		:title="item.title"
		:originalTitle="item.original_title"
		:language="item.original_language"
		:voteAverage="item.vote_average.toFixed(2)"
		:image="item.poster_path"
		:overview="item.overview"
		:id="item.id"
	  >
	  </Card>
	</div>
  </template>
  
  <script>
  import Card from "./Card.vue";
  
  export default {
	name: "Main",
	components: {
	  Card,
	},
	props: {
	  filmData: Array, // Accetta 'filmData' come proprietà, che è un array di dati dei film
	},
  };
  </script>
  
  <style lang="scss" scoped>
  </style>
  