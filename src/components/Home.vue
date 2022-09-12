<template>
  <v-app>
    <NavbarComponent />
    <v-main>
      <v-img class="mx-auto mt-10" width="650"
        src="https://www.androidiani.com/wp-content/uploads/2014/03/Pok%C3%A9mon_Gotta_Catch_Em_All_1.png">
      </v-img>
      <PokemonListComponent />
    </v-main>
  </v-app>
</template>
  
<script>

import NavbarComponent from './Navbar.vue'
import PokemonListComponent from './PokemonList.vue'

export default {
  name: 'HomeComponent',
  components: {
    NavbarComponent,
    PokemonListComponent,
  }
}

</script>
  