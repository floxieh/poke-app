<template>
    <div>
        <div class="grid-container cards">
            <div 
                :key="'poke' + index"
                v-for="(pokemon, index) in pokemons"
                class="card"
                @click="setPokemonUrl(pokemon.url)"
                >
                <div class="card-body shadow-lg text-center">
                    <img :src="imgUrl + pokemon.id + '.png'" class="card-img-top" alt="">
                    <p class="card-text">{{ pokemon.name }}</p>
                </div>
            </div>
        </div>
        <div class="text-center mt-3" id="scrolltrigger" ref="infinitescrolltrigger">
            <i class="fa fa-refresh fa-spin fa-2x fa-fw"></i>
            <span class="sr-only">Chargement...</span>
        </div>
    </div>
</template>

<script>
export default {
    props: [
        'apiUrl',
        'imgUrl'
    ],
    data() {
        return {
            pokemons: [],
            currentUrl: '',
            nextUrl: ''
        }
    },
    methods: {
        fetchData() {
            this.$http
                .get(this.currentUrl)
                .then((resp) => {
                    if(resp.status == 200) {
                        console.log("// Connection à l'API réussie //")
                        console.log(resp)
                        this.nextUrl = resp.data.next
                        resp.data.results.forEach(pokemon => {
                            pokemon.id = pokemon.url.split("/")[6]
                            this.pokemons.push(pokemon)
                        });
                        console.log("// Tableau de Pokémon //")
                        console.log(this.pokemons)
                    }
                })
                .catch((error) => {
                    console.log(error)
                })
        },
        scrollTrigger() {
            const observer = new IntersectionObserver(entries => {
                entries.forEach(entry => {
                    if(entry.intersectionRatio > 0 && this.nextUrl) {
                        console.log('In the view')
                        this.next()
                    } else {
                        console.log('Out the view')
                    }
                })
            })

            observer.observe(this.$refs.infinitescrolltrigger)
        },
        next() {
            this.currentUrl = this.nextUrl
            this.fetchData()
        },
        setPokemonUrl(url) {
            this.$emit('setPokemonUrl', url)
        }
    },
    created() {
        this.currentUrl = this.apiUrl
        this.fetchData()
    },
    mounted() {
        this.scrollTrigger()
    }
}
</script>

<style lang="scss">
    .grid-container {
        display: grid;
        // grid-template-columns: repeat(5, 1fr);
        // grid-template-rows: repeat(6, auto);
        grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
        gap: 10px 10px;

        @media (max-width: 1140px) {
            grid-template-columns: repeat(3, 1fr);
        }

        @media (max-width: 800px) {
            grid-template-columns: repeat(2, 1fr);
        }
    }

    .card {
        cursor: pointer;
    }

    .card-body img {
        width: 80px;
        height: 80px;
    }

    .card-text {
        font-weight: bold;
        text-transform: capitalize;
    }

    #scrolltrigger i {
        color: #ffffff;
    }
</style>