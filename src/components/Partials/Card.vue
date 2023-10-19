<template>
<!-- Contenitore della scheda -->
<div class="card-info col-2" @click="toggleCard">
    <!-- Il div "full" rappresenta il fronte della scheda -->
    <div class="full" :class="{ flipped: isFlipped }">
        <div class="card_front">
            <!-- Mostra l'immagine solo se è disponibile, altrimenti mostra un messaggio di nessuna immagine -->
            <img v-if="hasImage" class="img-fluid" :src="imagePath" alt="" />
        <div v-else class="no-image">Nessuna immagine disponibile</div>
        <div class="info">
            <h5 class="my-3">{{ title }}</h5>
            <div class="info_sub">
                <p>Lingua: {{ language }}<span :class="flagClass(language)"></span></p>
                <p>Media Voto: {{ formattedVoteAverage }}</p>
                <div class="rating">
                <!-- Crea fino a 5 stelle per la visualizzazione del voto -->
                <i v-for="i in 5" :key="i" :class="starClass(i)"></i>
                </div>
            </div>
        </div>
        </div>
        <!-- Il div "card_back" rappresenta il retro della scheda -->
        <div class="bg-black card_back">
            <div class="details">
            <!-- Mostra il titolo, il titolo originale e la media del voto -->
                <p><strong>Titolo:</strong>{{ title }}</p>
                <p><strong>Titolo originale:</strong>{{ originalTitle }}</p>
                <p><strong>Voto:</strong>{{ formattedVoteAverage }}</p>
                <p style="flex-direction: column"><strong>Trama:</strong>{{ overview }}</p>
            </div>
            <div class="actors">
            <!-- Mostra gli attori protagonisti -->
            <p><strong>Attori protagonisti:</strong>
                <span v-for="(actor, index) in actors" :key="index">{{ actor.name }}<i v-if="index < actors.length - 1">,</i></span>
            </p>
            </div>
            <div class="genres">
            <!-- Mostra i generi -->
                <p><strong>Genere/i:</strong>
                    <span v-for="(genre, index) in genres" :key="index">{{ genre.name }}<i v-if="index < genres.length - 1">,</i></span>
                </p>
            </div>
        </div>
    </div>
</div>
</template>
  
<script>
import { ref, computed, onMounted } from "vue";
import axios from "axios";
  
export default {
    name: "Card",
    props: {
        title: String,
        originalTitle: String,
        language: String,
        voteAverage: String,
        image: String,
        overview: String,
        id: Number,
    },
    setup(props) {
        const isFlipped = ref(false);
        const hasImage = computed(() => !!props.image);
        const imagePath = computed(() => hasImage.value ? `https://image.tmdb.org/t/p/w342/${props.image}` : "");
        const actors = ref([]);
        const genres = ref([]);
 
        // Formatta la media dei voti a due decimali
        const formattedVoteAverage = computed(() => parseFloat(props.voteAverage).toFixed(2));
  
        // Calcola le classi per le stelle in base alla media dei voti
        const starClass = (i) => {
            return i <= averageStar() ? 'fa-solid' : 'fa-regular';
        };
  
        // Calcola la media delle stelle basata sulla media dei voti
        const averageStar = () => {
            return Math.ceil(parseFloat(props.voteAverage) / 2);
        };
  
        // Calcola la classe della bandiera sulla base della lingua
        const flagClass = (language) => {
            return `fi fi-${language}`;
        };
  
        // Gestisce il flip della scheda quando viene cliccata
        const toggleCard = () => {
            isFlipped.value = !isFlipped.value;
        };
  
        // Carica gli attori e i generi utilizzando Axios dopo il montaggio del componente
        onMounted(() => {
            axios
            .get(`https://api.themoviedb.org/3/movie/${props.id}?api_key=540449cde6b90a1bf3ec549ebc9a2824`)
            .then((res) => {
                actors.value = res.data.credits;
                genres.value = res.data.genres;
            });
        });
  
        return {
            isFlipped,
            hasImage,
            imagePath,
            formattedVoteAverage,
            starClass,
            toggleCard,
            actors,
            genres,
            flagClass,
        };
    },
  };
</script>
  
<style lang="scss" scoped>
// Stili per la transizione flip
div.card-info:hover .full {
    transform: rotateY(180deg);
}
.full {
    height: 530px;
    width: 100%;
    position: relative;
    transition: all 1s linear;
    transform-style: preserve-3d;
    backface-visibility: hidden;
}
.card_front,
.card_back {
    height: 100%;
    width: 100%;
    border-radius: 20px;
    position: absolute;
}
.card_front {
    z-index: 2;
    backface-visibility: hidden;
}
.card_back {
    backface-visibility: hidden;
    padding: 20px;
    p {
        text-align: justify;
        display: flex;
        font-size: 0.8em;
        strong {
            display: inline;
            margin-right: 4px;
        }
    }
    z-index: 1;
    transform: rotateY(180deg);
}
.container {
    perspective: 1000px;
}
// Stili di base della scheda
div.card-info {
    background-color: transparent;
    perspective: 1000px;
    transition: 0.05s all ease-in-out;
    width: calc(100% / 5 - 10px);
    margin-top: 10px;
    box-shadow: -1px -1px 10px 1px rgba($color: #fff, $alpha: 0.3);
    border-radius: 20px;
    background-color: #000;
    color: #fff;
    text-align: center;
    img {
        border-radius: 20px 20px 0 0;
    }
    p {
        margin: 0;
        span {
            margin-left: 5px;
            border-radius: 3px;
        }
    }
}
.fi-en {
    background-image: url("../../node_modules/flag-icons/flags/4x3/gb.svg");
}
.fi-en.fis {
    background-image: url("../../node_modules/flag-icons/flags/1x1/gb.svg");
}
</style>
  