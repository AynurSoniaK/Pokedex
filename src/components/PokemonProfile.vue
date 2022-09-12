<template>
  <v-dialog v-model="dialog" width="500">
    <template v-slot:activator="{ on, attrs }">
      <v-btn @click="seePokemonFullProfile" v-bind="attrs" v-on="on" class="ma-4" color="#4165A9" dark>
        Voir plus
      </v-btn>
    </template>
    <v-card>
      <v-card-title class="text-h5 grey lighten-2 text-capitalize justify-center">
        {{ namePokemon }}
        <v-list-item-avatar class="mx-6 rounded-0" width="80px" height="80px">
          <v-img
            :src="`https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/versions/generation-v/black-white/animated/` + (pokemonFullProfile.id) +`.gif`">
          </v-img>
        </v-list-item-avatar>
      </v-card-title>
      <v-card-subtitle>
        <p>Abilities :
          <span class="mt-4" v-for="(ability, i) in pokemonFullProfile.abilities" :key="i">{{
          ability.ability.name }}</span>
        </p>
        <p>Types :
          <span class="mt-4" v-for="(type, i) in pokemonFullProfile.types" :key="i">{{
            type.type.name }}</span>
        </p>
        <p>Base experience : <span>{{pokemonFullProfile.base_experience}}</span> </p>
        <p>Weight : <span>{{pokemonFullProfile.weight}}</span> </p>
        <p>Height : <span>{{pokemonFullProfile.height}}</span> </p>
      </v-card-subtitle>
      <v-card-actions>
        <v-spacer></v-spacer>
        <v-btn color="primary" text @click="dialog = false">
          Close
        </v-btn>
      </v-card-actions>
    </v-card>
  </v-dialog>
</template>
    
<script>

import axios from 'axios';

export default {
  name: 'PokemonProfile',
  props: {
    namePokemon: String
  },
  data() {
    return {
      dialog: false,
      pokemonFullProfile: {}
    };
  },
  methods: {
    async seePokemonFullProfile() {
      try {
        const response = await axios.get(`https://pokeapi.co/api/v2/pokemon/${this.namePokemon}`)
        this.pokemonFullProfile = response.data
      }
      catch (err) {
        console.log(err)
      }
    },
  }
}

</script>

<style>
p {
  color: grey;
  font-size: 20px;
  font-weight: 700 
}

span {
  color: blue;
  font-weight: 300 
}

.v-dialog {
  border-radius: 10px 
}

.v-image__image--cover {
  background-size: contain 
}

.v-card__title {
  font-size: 100px;
  color:#202020
}

.v-card__subtitle {
  padding: 30px 20px 0px 20px !important
}
</style>
    