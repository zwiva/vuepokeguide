<template>
  <div class="main">
    <img class="logo" src="./assets/pokemon-logo.jpg" />
    <h1>PokeGuía</h1>

    <div>
      <label for="">Nombre:</label>
      <input type="text" v-model="pokemonName" />
      <button @click="searchPokemon(pokemonName)">Buscar</button>
    </div>

    <div>
      <!-- <img :src="urlImagen" alt=""> -->
      <h2>Movimientos</h2>
      <ul>
        <li v-for="(movimiento, index) of movimientos" :key="index">
          {{ movimiento }}
        </li>
      </ul>
      <h2>Habilidades</h2>
      <ul>
        <li v-for="(habilidad, index) of habilidades" :key="index">
          {{ habilidad }}
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
export default {
  components: {},
  name: "App",
  data: () => ({
    pokemonName: "",
    urlImagen: "",
    habilidades: [],
    movimientos: [],
  }),
  methods: {
    searchPokemon(pokemonName) {
      fetch(`https://pokeapi.co/api/v2/pokemon/${pokemonName}`)
        .then((response) => {
          return response.json();
        })
        .then((datosDelPokemon) => {
          console.log(datosDelPokemon);
        });
    },
    showPokemonDefault() {
      this.pokemonName = "pikachu";
      this.searchPokemon(this.pokemonName);
    },
  },
  created() {
    this.showPokemonDefault();
  },
  mounted() {
    console.log("componente montado");
  },
};
</script>

<style>
.main {
  text-align: center;
}
.logo {
  width: 50vh;
}
</style>
