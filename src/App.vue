<template>
  <v-app>
    <v-app-bar
      app
      color="amber accent-3"
      dark
    >
      <v-btn
        href="https://github.com/AynurSoniaK"
        target="_blank"
        text
      >
        Pokedex
      </v-btn>
    </v-app-bar>

    <v-main>
        <v-img 
            class="mx-auto mt-10"
            width="650"           
            src="https://www.androidiani.com/wp-content/uploads/2014/03/Pok%C3%A9mon_Gotta_Catch_Em_All_1.png">
        </v-img>
        <v-layout row wrap class="ma-10">
          <v-flex xs12 sm6 md4 lg3 v-for="(pokemon, i) in pokemonsList" :key="i">
              <v-card outlined flat class="text-center ma-5 rounded-xl">
                <v-card-text class="display-1">#{{ i+1 }}</v-card-text>
                <v-card-text class="text-capitalize display-1">{{ pokemon.name }}</v-card-text>
                <v-flex v-if="pokemonsDetails[i]">
                <v-chip class="mx-2 my-5" label outlined text-color="grey" v-for="(type, index) in pokemonsDetails[i].types" :key="index"><v-icon left>
                  mdi-label
                </v-icon> 
                  {{type.type.name}}
                </v-chip>
                </v-flex>
                <v-responsive>
                  <img 
                  height="150"
                  width="150"
                  :src="`https://img.pokemondb.net/artwork/large/` + (pokemon.name) +`.jpg`">
                </v-responsive>
              </v-card>
          </v-flex>
          <div 
            id="infiniteScroll"
            ref="infiniteScrollRef">
            <v-progress-circular 
              indeterminate
              class="mx-auto mt-10"
              color="grey"
            > 
            </v-progress-circular>
          </div>
        </v-layout>
    </v-main>
  </v-app>
</template>

<script>

import axios from 'axios'

export default {
  name: 'App',
  data () {
    return {
      pokemonsList: [],
      pokemonsDetails: [],
      nextList: [],
      currentList: [],
    }
  },
  mounted() {
      axios.get("https://pokeapi.co/api/v2/pokemon/").then((response) => {
        response.data.results.map( (pokemon) => {
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
          if(entry.intersectionRatio > 0 && this.nextList) {
            this.next()
          }
        })
      })
      observer.observe(this.$refs.infiniteScrollRef)
    },
    next() {
      this.current = this.nextList
      axios.get(this.nextList).then((response) => {
        response?.data.results.map( (pokemon) => {
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
    }
  }
}

</script>
