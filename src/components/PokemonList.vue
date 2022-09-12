<template>
  <div>
    <v-layout justify-center>
      <SearchComponent :getPokemonCardToParent="getPokemonCard" :getPokemonNameToParent="getPokemonName" />
    </v-layout>
    <v-layout row wrap class="ma-10">
      <v-row v-if="pokemonName">
        <v-flex xs12 sm6 md4 lg2>
          <div v-if="pokemonCard != `Not found`">
            <PokemonCard :index="pokemonCard.id" :name="pokemonCard.name" :types="pokemonCard.types" />
          </div>
          <div v-else style="min-width: 100vw">
            <p class="text-xl-h4 text-center">
              "{{pokemonName}}" was not found ! <br>
            </p>
          </div>
        </v-flex>
      </v-row>
      <v-row v-else>
        <v-flex xs12 sm6 md4 lg2 v-for="(pokemon, i) in pokemonsList" :key="i">
          <PokemonCard :index="i" :name="pokemon.name" :types="pokemonsDetails[i]?.types" />
        </v-flex>
        <v-row id="infiniteScroll" ref="infiniteScrollRef">
          <v-progress-circular indeterminate class="mx-auto mt-10" color="grey">
          </v-progress-circular>
        </v-row>
      </v-row>
    </v-layout>
  </div>
</template>
  
<script>

import axios from 'axios'
import PokemonCard from './PokemonCard.vue'
import SearchComponent from './SearchComponent.vue'


export default {
  name: 'PokemonListComponent',
  components: {
    PokemonCard,
    SearchComponent

  },
  data() {
    return {
      pokemonsList: [],
      pokemonsDetails: [],
      nextList: [],
      currentList: [],
      pokemonName: "",
      pokemonCard: ""
    }
  },
  mounted() {
    axios.get("https://pokeapi.co/api/v2/pokemon/").then((response) => {
      response.data.results.map((pokemon) => {
        this.pokemonsList.push(pokemon)
      })
      this.nextList = response.data.next
    }).then(() => {
      this.pokemonsList.forEach(pok => {
        axios.get(pok.url).then((response) => {
          this.pokemonsDetails.push(response.data)
        })
      });
    }
    )
    this.scroll()
  },
  methods: {
    scroll() {
      const observer = new IntersectionObserver((entries) => {
        entries.map(entry => {
          if (entry.intersectionRatio > 0 && this.nextList) {
            this.next()
          }
        })
      })
      observer.observe(this.$refs.infiniteScrollRef)
    },
    next() {
      this.current = this.nextList
      axios.get(this.nextList).then((response) => {
        response?.data.results.map((pokemon) => {
          this.pokemonsList.push(pokemon)
        })
        this.nextList = response.data.next
      }).then(() => {
        this.pokemonsList.forEach(pok => {
          axios.get(pok.url).then((response) => {
            this.pokemonsDetails.push(response.data)
          })
        });
      }
      )
    },
    getPokemonCard(card) {
      this.pokemonCard = card
      console.log(this.pokemonCard, "this.pokemonCard")
    },
    getPokemonName(name) {
      this.pokemonName = name
    }
  }
}

</script>
  