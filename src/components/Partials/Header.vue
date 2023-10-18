<template>
  <!-- Contenitore dell'intestazione -->
  <div class="header-container">
    <!-- Logo Netflix allineato a sinistra -->
    <img class="logo-netflix" :src="logoSource" :alt="logoAlt" />

    <!-- Barra di ricerca per scoprire film, serie, generi -->
    <!-- Emitte l'evento "updateSearch" alla pressione di un tasto -->
    <input
      v-bind:placeholder="searchPlaceholder"
      class="search-input form-control"
      type="text"
      name="search"
      id="searchInput"
      v-model="searched"
      @keyup="onSearchKeyup"
    />

    <!-- Menù a tendina per selezionare il genere -->
    <!-- Emitte l'evento "filterGenre" al cambio del valore selezionato -->
    <!-- Il valore selezionato è associato alla variabile "genreId" -->
    <select
      @change="onGenreChange"
      v-model="genreId"
      class="genre-select form-select"
      :aria-label="ariaLabel"
      id="genreSelect"
      name="genreSelect"
    >
      <!-- Opzione predefinita "Tutti i generi" -->
      <option 
        :value="defaultGenreValue" 
        :selected="true">
        {{ defaultGenreLabel }}
      </option>
      <!-- Genera opzioni dinamiche basate sull'array "genre" -->
      <option 
        :key="item.id" 
        v-for="item in genre" 
        :value="item.id">
        {{ item.name }}
      </option>
    </select>
  </div>
</template>

<script>
export default {
  name: "Header",
  props: {
    genre: Array, // Prop: Array di generi per il menù a tendina
  },
  data() {
    return {
      searched: "", // Testo nella barra di ricerca
      genreId: "", // Genere selezionato nel menù a tendina
    };
  },
  computed: {
    // Logo Netflix
    logoSource() {
      return "https://image.tmdb.org/t/p/w200/wwemzKWzjKYJFfCeiB57q3r4Bcm.png";
    },
    // Logo Netflix
    logoAlt() {
      return "Logo Netflix";
    },
    // Placeholder della barra di ricerca
    searchPlaceholder() {
      return "Cerca film, serie, generi...";
    },
    // Aria-label della barra di ricerca
    ariaLabel() {
      return ".form-select-lg example";
    },
    // Valore predefinito per il genere "Tutti i generi"
    defaultGenreValue() {
      return "";
    },
    // Label predefinito per il genere "Tutti i generi"
    defaultGenreLabel() {
      return "Tutti i generi";
    },
  },
  methods: {
    // Gestisce l'evento di pressione di un tasto nella barra di ricerca
    onSearchKeyup() {
      this.$emit("updateSearch", this.searched);
      console.log(this.searched);
    },
    // Gestisce l'evento di cambio del valore nel menù a tendina
    onGenreChange() {
      this.$emit("filterGenre", this.genreId);
      console.log(this.genreId);
    },
  },
};
</script>

<style lang="scss" scoped>
.header-container {
  background-color: rgba(139, 26, 26, 0.513); // Sfondo nero
  padding: 15px 0; // Padding superiore e inferiore
  display: flex;
  justify-content: space-between;
  align-items: center;

  .logo-netflix {
    margin-right: auto; // Margin a destra per il logo Netflix
    @media screen and (max-width: 400px) {
      display: none; // Nascondi il logo su schermi con larghezza <= 400px
    }
  }

  .search-input,
  .genre-select {
    width: 25%; // Larghezza iniziale
    margin: 0 5px; // Margini
    @media screen and (max-width: 400px) {
      width: 50%; // Riduci la larghezza su schermi con larghezza <= 400px
    }
  }
}
</style>
