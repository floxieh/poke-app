<template>
    <div>
        <form 
            class="input-group mb-3"
            @submit.prevent="setPokemonUrl"
        >
            <input 
                type="text" 
                class="form-control" 
                placeholder="Find a Pokemon" 
                v-model="searchvalue"
                @input="searchvalue = $event.target.value.toLowerCase()"
            >
        </form>
    </div>
</template>

<script>
export default {
    props: [
        'apiUrl'
    ],
    data() {
        return {
            searchvalue: ''
        }
    },
    methods: {
        setPokemonUrl() {
            if(this.searchvalue !== '') {
                this.$emit('setPokemonUrl', this.apiUrl + this.searchvalue)
            }
        }
    }
}
</script>

<style scoped>
    form {
        z-index: 0;
    }
</style>