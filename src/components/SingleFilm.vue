<script>
export default {
    name: 'SingleFilm',
    props: ['info'],
    data() {
        return {
            hover: false,
            // Array of flags
            flags: [
                {
                    img: 'img/france.png',
                    lang: "fr"
                },
                {
                    img: 'img/german-flag.png',
                    lang: "de"
                },
                {
                    img: 'img/italy.png',
                    lang: "it"
                },
                {
                    img: 'img/united-kingdom.png',
                    lang: "en"
                },
                {
                    img: 'img/spain.png',
                    lang: "es"
                },
                {
                    img: 'img/unflag.png',
                    lang: ""
                },
            ]
        }
    },
    computed: {
        flagEmoji() {

            for (let i = 0; i < this.flags.length; i++) {
                if (this.flags[i].lang.includes(this.info.original_language)) {
                    return this.flags[i].img
                }
            }
            return this.flags[this.flags.length - 1].img
        },
        rateFilm() {
            return Math.round(this.info.vote_average / 2);
        }
    }
}   
</script>

<template>
    <div class="cards-hover">
        <img :src="`https://image.tmdb.org/t/p/w342/${info.poster_path}`" alt="">

        <!-- Details of card -->
        <div class="details">
            <!-- template di card singola -->
            <div>
                <span>
                    <strong> Titolo:</strong>
                    {{ info.title }}
                </span>
            </div>
            <div>
                <strong> Titolo Originale:</strong>
                {{ info.original_title }}
            </div>

            <!-- Flags -->
            <!-- se nel loop, il flag nell'array include original_langue allora RETURN l'array immagine in base al lang (PS. il lang dell'array deve essere uguale al lang dell'API) -->
            <div class="flags">
                <span> Language:</span>
                <img :src="flagEmoji" alt="">
            </div>

            <!-- Stars [ star in rateFilm ( nella quale il voto da 10 diventa 5)] -->
            <div class="stars">
                <span>Voto:</span>

                <div v-for="star in rateFilm">
                    <font-awesome-icon icon="fa-solid fa-star" class="yellowstar" />
                </div>

                <div v-for="star in 5 - rateFilm">
                    <font-awesome-icon icon="fa-solid fa-star" class="blackstar" />
                </div>
            </div>

            <div class="my-5">
                <strong>Overview: </strong>
                {{ info.overview }}
            </div>
        </div>

    </div>
</template>

<style lang="scss">
@use "../styles/partials/variables.scss" as *;

.cards-hover {
    position: relative;
}

.stars {
    display: flex;

    .yellowstar {
        color: yellow;
    }

    .blackstar {
        color: lightgrey;
    }
}

.details {
    width: 100%;
    height: 100%;
    display: none;
    position: absolute;
    background-color: rgba(0, 0, 0, 0.439);
    color: white;
    top: 0;
    left: 0;
    padding: 30px;
    overflow: auto;
}

.cards-hover:hover .details {
    display: block;
}

.flags {
    margin-top: 10px;
    margin-bottom: 10px;

    img {
        margin-left: 5px;
        width: 30px;
    }
}
</style>