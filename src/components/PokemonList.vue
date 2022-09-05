<template>
    <v-main>
    </v-main>
</template>

<script>

import axios from 'axios'

export default {
  name: 'App',

  mounted() {
      axios.get("https://pokeapi.co/api/v2/pokemon/").then((response) => {
        console.log(response.data)
      })
  }
}

</script>
