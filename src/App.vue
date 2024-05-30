<template>
  <div id="app" class="background">
    <div class="container">
    <header class="text-center">
      <img class="w-50 h-25" src="./assets/Pokemon.svg" alt="" />
      <img src="./assets/Pokemon.png" alt="">
      <h3 class="my-4">Pokemones descubiertos: <span>{{ contador }}</span></h3>
    </header>
    <div class="row g-3">
      <div class="col-3" v-for="(pokemon, index) in pokemones" :key="index">
        <!-- props (Pokemon) pokemon(valor) -->
        <CardPoke :pokemon="pokemon"
        @respuestaCorrecta = "incrementar" />
        <pokemonDescubierto/>
      </div>
    </div>
    </div>
  </div>
</template>

<script>
import CardPoke from "./components/CardPoke.vue";
import axios from "axios";

export default {
  name: "App",
  components: {
    CardPoke,
  },
  data() {
    return {
      pokemones: [],
      contador:0,
    };
  },
  methods: {
    async getPoke() {
      try {
        let response = await axios.get("https://pokeapi.co/api/v2/pokemon");
        // console.log(response);
        this.pokemones = response.data.results;
      } catch (error) {
        console.log(error);
      }
    },
    incrementar(){
      this.contador ++
    },
  },
  mounted() {
    this.getPoke();
  },
};
</script>

<style scoped>
#app{
  font-family: "Space Mono", monospace;
   -webkit-text-stroke: 2px blue;
 
}
h3 {
  -webkit-text-stroke: 2px blue; /* ancho borde y color del borde */
  color: yellow;
  
 
}
.background{
  background-position: center;
  background-attachment: fixed;
  background-image: url("../src/assets/fondo.jpg");
  background-size: cover;
}

</style>
