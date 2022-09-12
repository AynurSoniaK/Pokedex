<template>
  <v-card outlined flat class="text-center ma-5 rounded-xl">
    <v-card-text class="display-1">#{{ index+1 }}</v-card-text>
    <v-card-text class="text-capitalize display-1">{{ name }}</v-card-text>
    <v-chip v-for="(type, i) in types" :key="i" class="mb-5 pa-2 type" label outlined text-color="grey">
      <v-icon left :class="type.type.name">
        mdi-label
      </v-icon>
      <span :class="type.type.name">
        {{ type.type.name.toUpperCase() }}
      </span>
    </v-chip>
    <v-responsive>
      <img height="150" width="150" :src="`https://img.pokemondb.net/artwork/large/` + (name) +`.jpg`">
    </v-responsive>
    <PokemonProfile :namePokemon="name"/>
  </v-card>
</template>
  
<script>

import PokemonProfile from "./PokemonProfile.vue"

export default {
  name: 'PokemonCard',
  components: {
    PokemonProfile
  },
  props: {
    index: Number,
    name: String,
    types: Array,
  },
  computed: {
    setColor() {
      return (item) => {
        if (item === "grass") {
          this.isColor = 'red';
        } else {
          this.isColor = 'blue';
        }
        return { color: this.isColor };
      }
    }
  }
}

</script>
<style lang="scss" scoped>
.type {
  border: none;
  .grass {
    color: #6ac80b;
  }
  .poison {
    color: #e37c1c;
  }
  .water {
    color: #236ef9;
  }
  .dragon {
    color: #a8032b;
  }
  .ice {
    color: #23f9f9;
  }
  .flying {
    color: #cca804;
  }
  .fire {
    color: #d93108;
  }
  .ghost {
    color: #d050ed;
  }
  .fighting {
    color: #a51770;
  }
  .normal {
    color: #585858;
  }
  .psychic {
    color: #f18ebe,
  }
  .bug {
    color: #2e5b17;
  }
  .dark {
    color: #262726,
  }
  .steel {
    color: #5d5f5c;
  }
  .fairy {
    color: #FFB6C1;
  }
  .eletric {
    color: #FFEA00;
  }
  .rock {
    color: #5A4D41
  }
  .ground {
    color: #A52A2A;
  }
}
</style>
