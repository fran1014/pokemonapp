<template>
  <h1 v-if="!pokemon">Esper porfavor...</h1>
  <div v-else>
    <h1>Which one is this pokemon?</h1>
    <PokemonPicture :pokemon-id="pokemon.id" :showPokemon="showPokemon" />
    <PokemonOptions :pokemons="pokemonArr" @selection-pokemon="checkAnswer" />

    <h2>{{ message }}</h2>
    <button>Nuevo Juego</button>
  </div>
</template>

<script>
import PokemonOptions from '@/components/PokemonOptions';
import PokemonPicture from '@/components/PokemonPicture';

import getPokemonOptions from '@/helpers/getPokemonOptions';

getPokemonOptions();

export default {
  components: { PokemonOptions, PokemonPicture },
  data() {
    return {
      pokemonArr: [],
      pokemon: null,
      showPokemon: false,
      showAnswer: false,
      message: '',
    };
  },
  methods: {
    async mixPokemonArray() {
      this.pokemonArr = await getPokemonOptions();

      const rdnInt = Math.floor(Math.random() * 4);
      this.pokemon = this.pokemonArr[rdnInt];
    },
    checkAnswer(selectedId) {
      this.showPokemon = true;

      if (selectedId === this.pokemon.id) {
        this.message = `Correcto, ${this.pokemon.name} `;
      } else {
        this.message = `Oopps, era ${this.pokemon.name}`;
      }
    },
  },

  mounted() {
    this.mixPokemonArray();
  },
};
</script>
