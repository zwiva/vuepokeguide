<template>
  <div class="main">
    <img class="logo" src="./assets/pokelogo.svg" />
    <h1>PokeGuía</h1>

    <div>
      <label for="">Nombre: </label>
      <input
        type="text"
        v-model="pokemonName"
        @click="clearInput()"
        class="search-input"
      />
      <button @click="searchPokemon(pokemonName)" class="search-button">
        Buscar
      </button>
    </div>

    <div v-if="this.successfulSearch">
      <div class="screen__view">
        <img :src="urlImagen" alt=""/>
      </div>
      <h2>Movimientos</h2>
      <ul>
        <li v-for="(eachmove, index) of getMoves" :key="index">
          {{ eachmove.move.name }}
        </li>
      </ul>

      <h2>Habilidades</h2>
      <ul>
        <li v-for="(eachability, index) of getAbilities" :key="index">
          {{ eachability.ability.name }}
        </li>
      </ul>
    </div>
    <div v-if="this.errorSearch">
      <h2 class="error-search">Ups, ese Pokemon no existe! <img src="./assets/nursejoy.gif" alt=""></h2>
    </div>
  </div>
</template>

<script>
export default {
  components: {},
  name: "App",
  data: () => ({
    pokemonName: "",
    pokemonData: [],
    urlImagen: "",
    successfulSearch: false,
    errorSearch: false,
  }),
  methods: {
    async searchPokemon(pokemonName) {
      try {
        let response = await fetch(
          `https://pokeapi.co/api/v2/pokemon/${pokemonName}`
        );
        let allPokemonData = await response.json();

        this.pokemonData = allPokemonData;
        console.log(this.pokemonData);
        this.urlImagen = this.pokemonData.sprites.front_default;
        this.successfulSearch = true;
        this.errorSearch = false;

        // fetch(`https://pokeapi.co/api/v2/pokemon/${pokemonName}`)
        //   .then((response) => {
        //     return response.json();
        //   })
        //   .then((allPokemonData) => {
        //     this.pokemonData = allPokemonData;
        //     console.log(this.pokemonData);
        //     this.urlImagen = this.pokemonData.sprites.front_default;
        //     this.successfulSearch = true;
        //   });
      } catch (error) {
        this.errorSearch = true;
        console.log(error);
      }
    },
    showPokemonDefault() {
      this.pokemonName = "pikachu";
      this.searchPokemon(this.pokemonName);
    },
    clearInput() {
      this.errorSearch = false;
      this.pokemonName = "";
      this.pokemonData = "";
      this.successfulSearch = false;
    },
  },
  created() {
    this.showPokemonDefault();
  },
  computed: {
    getMoves() {
      console.log("moves", this.pokemonData.moves);
      return this.pokemonData.moves;
    },

    getAbilities() {
      console.log("abilities", this.pokemonData.abilities);
      return this.pokemonData.abilities;
    },
  },
};
</script>

<style>
body {
  margin: 0;
  padding: 0;
  background-color: red;
  color: white;
}
.main {
  text-align: center;
}
.logo {
  width: 90%;
  max-width: 50vh;
}
ul {
  list-style-type: none;
}
h1,
h2 {
  color: #3d7dca;
  text-shadow: 2px 2px #003a70;
  background-color: #ffcb05;
}
.search-input {
  border: none;
  padding: 1em;
  color: gray;
}
.search-button {
  background-color: green;
  padding: 1em;
  border: none;
  border-radius: 0.25em;
  margin: 1em;
  color: white;
  font-weight: bold;
}
.screen__view {
  width: 100px;
  border: solid 1.5em rgb(208, 206, 206);
  border-radius: 5px;
  background: linear-gradient(
    0deg,
    rgba(173, 255, 46, 1) 0%,
    rgba(103, 252, 255, 1) 100%
  );
  margin: 1em auto;
}
.error-search {
  color: #d0a910;
  text-shadow: 2px 2px #8d7308;
  padding: 1em;
}
.error-search>img{
  height: 70px;
}
</style>
