<template>
    <div class="my-9">
        <v-toolbar class="py-3 my-9" width=300 height=60 rounded>
            <v-text-field flat prepend-icon="mdi-magnify" placeholder="Look for someone here ..." v-model="pokemonName">
            </v-text-field>
        </v-toolbar>
        <v-row>
            <v-btn class="mr-2" @click="searchPokemon">Search Pokémon</v-btn>
            <v-btn @click="resetInputSearch">Clear search</v-btn>
        </v-row>
    </div>
</template>
    
<script>

import axios from 'axios'

export default {
    name: 'SearchComponent',
    data() {
        return {
            pokemonCard: null,
            pokemonName: "",
        }
    },
    props: {
        getPokemonCardToParent: Function,
        getPokemonNameToParent: Function
    },
    methods: {
        async searchPokemon() {
            try {
                const pokemonSearched = await axios.get(`https://pokeapi.co/api/v2/pokemon/${this.pokemonName.toLowerCase()}`)
                this.pokemonCard = pokemonSearched.data
                this.getPokemonCardToParent(this.pokemonCard)
                this.getPokemonNameToParent(this.pokemonName)
            }
            catch (err) {
                this.pokemonCard = "Not found"
                this.getPokemonCardToParent(this.pokemonCard)
                this.getPokemonNameToParent(this.pokemonName)
            }

        },
        resetInputSearch() {
            this.pokemonName = ""
            this.getPokemonNameToParent("")
        }
    }
}

</script>
    