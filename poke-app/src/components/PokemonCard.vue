<template>
    <div class="pokemon-card">
      <img
        :src="pokemon.image"
        :alt="pokemon.name"
        :style="{'filter': pokemon.isDiscovered ? 'none' : 'blur(5px) grayscale(100%)'}"
      />
      <input
        v-if="!pokemon.isDiscovered"
        v-model="guess"
        @keyup.enter="checkGuess"
      />
      <button v-if="!pokemon.isDiscovered" @click="checkGuess">Descubrir</button>
      <p v-if="pokemon.isDiscovered">{{ pokemon.name }}</p>
    </div>
  </template>
  
  <script>
  export default {
    name: 'PokemonCard',
    props: {
      pokemon: Object,
    },
    data() {
      return {
        guess: '',
      };
    },
    methods: {
      checkGuess() {
        if (this.guess.toLowerCase() === this.pokemon.name.toLowerCase()) {
          this.$emit('guessed', this.pokemon, true); 
        } else {
          alert('Upss 😩 ¡Intenta de nuevo!');
          this.guess = ''; 
        }
      },
    },
  };
  </script>
  
  <style>
  .pokemon-card {
    margin: 10px;
    text-align: center;
  }
  </style>
  