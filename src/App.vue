<template>
  <div id="app">
    <NavbarC @enviar-para-pai="receberDoFilho" :metodoPai="buscar" />
    <hr />
    <div class="column is-half is-offset-one-quarter">
      <div v-for="(poke, index) in filteredPokemons" :key="poke.url">
        <PokemonC :name="poke.name" :url="poke.url" :num="index + 1" />
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import PokemonC from "./components/PokemonC.vue";
import NavbarC from "./components/NavbarC.vue";

export default {
  name: "App",
  data() {
    return {
      pokemons: [],
      filteredPokemons: [],
      valorRecebido: null,
    };
  },
  created: function () {
    axios
      .get("https://pokeapi.co/api/v2/pokemon?limit=151&offset=0")
      .then((res) => {
        this.pokemons = res.data.results;
        this.filteredPokemons = res.data.results;
        console.log("Lista de Pokemons capturada!");
      })
      .catch((err) => {
        console.warn(err);
      });
  },
  components: {
    PokemonC,
    NavbarC,
  },
  methods: {
    buscar() {
      console.log('buscar chamado')
      if (this.valorRecebido !== null && this.valorRecebido !== undefined) {
        const valorSemEspacos = this.valorRecebido.trim();

        if (valorSemEspacos === "") {
          this.filteredPokemons = this.pokemons;
        } else {
          this.filteredPokemons = this.pokemons.filter(
            (pokemon) =>
              pokemon.name.toLowerCase() === valorSemEspacos.toLowerCase()
          );
        }
      }
      console.log('Lista de pokémons após a busca:', this.filteredPokemons);
    },

    receberDoFilho(valor) {
      this.valorRecebido = valor;
      console.log("Valor Recebido do filho:", valor);
      this.buscar(); // Chama a função de busca
    },
  },
};
</script>

<style>
</style>
