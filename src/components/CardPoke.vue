<template>
  <div class="container text-center mb-3">
    <div class="border-0" style="width: ">
      <!-- Hicimos biding de clases -->
      <img
        :src="imagenPokemon"
        class="card-img-top"
        :class="esconderPokemon ? 'borroso' : ''"
        alt=""
      />
      <div class="card-body">
        <p v-show="!esconderPokemon">{{ pokemon.name }}</p>
        <h5 class="card-title"></h5>
        <p class="card-text"></p>
        <form v-show="esconderPokemon">
          <input
            class="form-control my-3"
            type="text"
            v-model="descubrirPokemon"
            @keyup.enter="descubrir"
          />

          <div class="d-grid rounded">
            <button button @click.prevent="descubrir" class="btn btn-lg">
              Descubrir
            </button>
          </div>
        </form>
      </div>
    </div>
  </div>
</template>
<script>
import axios from "axios";

export default {
  name: "CardPoke",
  props: {
    pokemon: Object,
  },
  data() {
    return {
      infoPokemon: {},
      esconderPokemon: true,
      descubrirPokemon: "",
    };
  },
  computed: {
    imagenPokemon() {
      return this.infoPokemon.sprites?.other["official-artwork"].front_default;
    },
  },

  // Ciclo de vida de los componentes (ejecuta lo que esta dentro de la función created)
  created() {
    this.getInfoPokemon();
  },
  methods: {
    async getInfoPokemon() {
      try {
        //  destructurar {data}
        let { data } = await axios.get(this.pokemon.url);
        this.infoPokemon = data;
      } catch (error) {
        console.log(error);
      }
    },
    // .toLowerCase (convierte texto en minuscula)
    descubrir() {
      if (
        this.descubrirPokemon.toLowerCase() === this.pokemon.name.toLowerCase()
      ) {
        this.esconderPokemon = false;
        this.$emit("respuestaCorrecta");
        console.log("correcto");
      } else {
        console.log("incorrecto");
        const name = this.pokemon.name;
        this.$swal.fire({
          title: "Ups!",
          text: `Intenta nuevamente. el nombre del pokemon comienza con ${name.substring(
            0,
            3
          )}`,
          icon: "error",
          confirmButtonText: "Continuar",
        });
      }
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.card-body {
  font-family: "Space Mono", monospace;
  text-transform: capitalize;
}
.borroso {
  filter: blur(5px) grayscale(100%);
}
.btn {
  color: rgb(22, 34, 104);
}
.d-grid {
  background-color: rgb(243, 208, 52);
}
/* style que no permite que se muevan las imagenes */
img {
  
  -webkit-user-drag: none;
  user-select: none;
  -moz-user-select: none;
  -webkit-user-select: none;
  -ms-user-select: none;
}
</style>
