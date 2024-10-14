<template>
  <div id="app">
    <img class="logoPokemon" src="../src/assets/logopokemon.png" alt="logo pokemon">
    <h1 style="text-align: center;">Adivina el Pokémon</h1>
    <p style="text-align: center;">Pokémons descubiertos: {{ discoveredPokemons }}</p>
    <div class="pokemon-container">
      <PokemonCard
        v-for="pokemon in pokemons"
        :key="pokemon.name"
        :pokemon="pokemon"
        @guessed="handleGuess"
      />
    </div>
  </div>
</template>

<script>
import axios from 'axios';
import PokemonCard from './components/PokemonCard.vue';

export default {
  name: 'App',
  components: {
    PokemonCard,
  },
  data() {
    return {
      pokemons: [],
      discoveredCount: 0,
    };
  },
  computed: {
    discoveredPokemons() {
      return this.pokemons.filter(pokemon => pokemon.isDiscovered).length;
    },
  },
  methods: {
    handleGuess(pokemon, isCorrect) {
      if (isCorrect) {
        pokemon.isDiscovered = true; // Marca el Pokémon como descubierto
        this.discoveredCount++;
      }
    },
  },
  async mounted() {
    try {
      const response = await axios.get('https://pokeapi.co/api/v2/pokemon?limit=20');
      const promises = response.data.results.map(pokemon =>
        axios.get(pokemon.url)
      );
      const pokemonResponses = await Promise.all(promises);
      this.pokemons = pokemonResponses.map(res => ({
        name: res.data.name,
        image: res.data.sprites.front_default,
        isDiscovered: false, // Inicialmente no descubierto
      }));
    } catch (error) {
      console.error('Error fetching Pokémon:', error);
    }
  },
};
</script>



<style>
.pokemon-container {
  display: flex;
  flex-wrap: wrap;
  
}
.logoPokemon {
  display: block; 
  margin: 0 auto; 
}
</style>
