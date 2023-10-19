<template>
  <div id="app">
    <!-- Barra di ricerca e filtro generi per l'utente -->
    <Header @updateSearch="updateSearch" @filterGenre="filterGenre" :genre="genre" />

    <main class="container-xl mt-3">
      

      <!-- Componente Main per la visualizzazione dei film -->
      <Main :filmData="genreSearchMovie" />

      

      <!-- Mostra il titolo della sezione Serie TV solo se è stata effettuata una ricerca -->
      <h2 v-if="selected.text != ''" class="mt-4">Serie TV</h2>

      

      
    </main>
  </div>
</template>

<script>
import "flag-icons";
import "bootstrap";
import axios from "axios";
import Header from "./components/Partials/Header.vue";
import Main from "./components/Partials/Main.vue";

export default {
  name: "App",
  components: {
    Header,
    Main,
  },
  data() {
    return {
      // Chiave API per The Movie Database
      apiKey: "540449cde6b90a1bf3ec549ebc9a2824",

      // URL per la ricerca di film
      movieSearch: "https://api.themoviedb.org/3/search/movie/",

      // URL per la ricerca di Serie TV
      showsSearch: "https://api.themoviedb.org/3/search/tv/",

      // URL per i film popolari
      popular: "https://api.themoviedb.org/3/trending/movie/day?api_key=540449cde6b90a1bf3ec549ebc9a2824",

      // Oggetto per memorizzare le scelte dell'utente
      selected: {
        lang: "it-IT", // Imposta la lingua italiana
        text: "", // Inizialmente, non c'è testo di ricerca
        genre: "", // Inizialmente, nessun genere selezionato
      },

      // Elenco dei generi
      genre: null,

      // Risultati della ricerca di film
      filmData: [],

      
      

      toGet: "", // Non utilizzato

      data: null, // Non utilizzato

      headingTop: null, // Inizialmente, il titolo principale è vuoto
    };
  },
  created() {
    // Inizializza i dati quando la pagina viene caricata
    this.getPopular(); // Ottiene i film popolari
    this.getGenreList(); // Ottiene l'elenco dei generi
  },
  methods: {
    // Funzione per aggiornare la ricerca di film
    updateSearch(search) {
      this.selected.text = search;
    },

    // Funzione per filtrare per genere
    filterGenre(genre) {
      this.selected.genre = genre;
    },

    // Funzione per ottenere i film popolari
    getPopular() {
      this.headingTop = "Popolari questa settimana";
      axios.get(this.popular).then((res) => {
        this.filmData = res.data.results; // Aggiorna i risultati dei film popolari
      });
    },

    // Funzione per ottenere i dati dei film in base alla ricerca dell'utente
    getFilmData() {
      axios
        .get(
          this.movieSearch + this.apiKey + "&query=" + this.selected.text + "&append_to_response=credits"
        )
        .then((res) => {
          this.filmData = res.data.results; // Aggiorna i risultati dei film in base alla ricerca
        });
    },

    
    

    // Funzione per ottenere la lista dei generi
    getGenreList() {
      axios
        .get("https://api.themoviedb.org/3/genre/movie/list?api_key=540449cde6b90a1bf3ec549ebc9a2824")
        .then((res) => {
          this.genre = res.data.genres; // Aggiorna l'elenco dei generi per i film
        });

      axios
        .get("https://api.themoviedb.org/3/genre/tv/list?api_key=540449cde6b90a1bf3ec549ebc9a2824")
        .then((res) => {
          let data = res.data.genres;
          data.forEach((element) => {
            // Filtra i generi duplicati
            let test = this.genre.filter((g) => g.id == element.id);
            if (test.length == 0) {
              this.genre.push(element);
            }
          });
        });
    },
  },
  watch: {
    // Controlla se è stata effettuata una ricerca o se la barra di ricerca è vuota
    "selected.text": function () {
      if (this.selected.text == "") {
        this.headingTop = "I titoli del momento";
        this.getPopular(); // Ottiene i film popolari se la ricerca è vuota
      } else {
        this.headingTop = "Film";
        this.getFilmData(); // Ottiene i dati dei film in base alla ricerca
        
      }
    },
  },
  computed: {
    // Filtra i film in base al genere selezionato
    genreSearchMovie: function () {
      if (this.selected.genre != "") {
        return this.filmData.filter((film) => film.genre_ids.includes(this.selected.genre));
      } else {
        return this.filmData; // Restituisce tutti i film se nessun genere è selezionato
      }
    },
    
  },
};
</script>

<style lang="scss">
@import "bootstrap/dist/css/bootstrap.min.css";
div#app {
  background: linear-gradient(#000, #6e0d14);
  min-height: 100vh;
  main {
    padding-bottom: 60px;
    h2 {
      padding-left: 5%;
      color: #fff;
      font-weight: bold;
    }
  }
  .no-result {
    color: #fff;
    opacity: 0.8; // Opacità del messaggio "Nessun risultato per film"
    margin: 10px 100px;
  }
}
</style>