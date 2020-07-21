<template>
    <div class="container">
        <div class="header mt-3 mb-5 mx-auto text-center">
            <img src="../assets/pokemon_logo.png" alt="Pokemon Logo">
        </div>

        <PokeSearch
            :apiUrl="apiUrl"
            @setPokemonUrl="setPokemonUrl"
        />

        <PokeList 
            :apiUrl="apiUrl"
            :imgUrl="imgUrl"
            @setPokemonUrl="setPokemonUrl"
        />
        
        <transition leave-active-class="animate__animated animate__fadeOut">
            <PokeDetail 
                v-if="showDetail"
                :pokeUrl="pokeUrl"
                :imgUrl="imgUrl"
                @closeDetail="closeDetail"
            />
        </transition>
    </div>
</template>

<script>
import PokeSearch from './PokeSearch.vue'
import PokeList from './PokeList.vue'
import PokeDetail from './PokeDetail.vue'

export default {
    data() {
        return {
            apiUrl: 'https://pokeapi.co/api/v2/pokemon/',
            imgUrl: 'https://pokeres.bastionbot.org/images/pokemon/',
            pokeUrl: '',
            showDetail: false
        }
    },
    methods: {
        setPokemonUrl(url) {
           this.pokeUrl = url
           console.log('// Url du Pokémon en détail //') 
           console.log(this.pokeUrl) 
           this.showDetail = true
        },
        closeDetail() {
            this.showDetail = false
        }
    },
    components: {
        PokeSearch,
        PokeList,
        PokeDetail
    }
}
</script>

<style lang="scss">
    body {
        background-image: linear-gradient(45deg, #ff9a9e 0%, #fad0c4 99%, #fad0c4 100%);
        width: calc(100% - 20px);
        min-height: calc(100vh - 20px);
    }

    .header img {
        width: 300px;
        max-width: 100%;
        }

</style>