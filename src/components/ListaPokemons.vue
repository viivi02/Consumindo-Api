<template>
  <div class="pokemon-list-container">
    <h1 class="title">Lista de Pokémons</h1>
    <ul class="pokemon-list">
      <li v-for="pokemon in pokemons" :key="pokemon.name" @click="selectPokemon(pokemon)">
        <img v-if="pokemon.sprites" :src="pokemon.sprites.front_default" :alt="pokemon.name" width="50" />
        <span>{{ pokemon.name }}</span>
      </li>
    </ul>

    <PokemonModal :visible="showModal" :typeColor="typeColor" @close="closeModal">
      <div v-if="selectedPokemon">
        <h2>Detalhes do Pokémon</h2>
        <img :src="selectedPokemon.sprites.front_default" :alt="selectedPokemon.name" width="100" />
        <p><strong>Nome:</strong> {{ selectedPokemon.name }}</p>
        <p><strong>Tipo:</strong> {{ formatTypes(selectedPokemon.types) }}</p>
      </div>
    </PokemonModal>
  </div>
</template>

<script>
import axios from 'axios';
import PokemonModal from './PokemonsModal.vue';

export default {
  components: {
    PokemonModal
  },
  data() {
    return {
      pokemons: [],
      selectedPokemon: null,
      showModal: false,
      typeColor: ''
    };
  },
  created() {
    this.fetchPokemons();
  },
  methods: {
    async fetchPokemons() {
      try {
        const response = await axios.get('https://pokeapi.co/api/v2/pokemon?limit=20');
        const pokemons = response.data.results;
        const detailedPokemons = await Promise.all(pokemons.map(async (pokemon) => {
          const details = await axios.get(pokemon.url);
          return { ...pokemon, sprites: details.data.sprites, types: details.data.types };
        }));

        this.pokemons = detailedPokemons;
      } catch (error) {
        console.error('Erro ao buscar pokémons:', error);
      }
    },
    async selectPokemon(pokemon) {
      try {
        const response = await axios.get(pokemon.url);
        this.selectedPokemon = response.data;
        this.typeColor = this.getTypeColor(this.selectedPokemon.types[0].type.name);
        this.showModal = true;
      } catch (error) {
        console.error('Erro ao buscar detalhes do pokémon:', error);
      }
    },
    closeModal() {
      this.showModal = false;
      this.selectedPokemon = null;
    },
    formatTypes(types) {
      return types.map(typeInfo => typeInfo.type.name).join(', ');
    },
    getTypeColor(type) {
      const typeColors = {
        normal: '#A8A77A',
        fire: '#EE8130',
        water: '#6390F0',
        electric: '#F7D02C',
        grass: '#7AC74C',
        ice: '#96D9D6',
        fighting: '#C22E28',
        poison: '#A33EA1',
        ground: '#E2BF65',
        flying: '#A98FF3',
        psychic: '#F95587',
        bug: '#A6B91A',
        rock: '#B6A136',
        ghost: '#735797',
        dragon: '#6F35FC',
        dark: '#705746',
        steel: '#B7B7CE',
        fairy: '#D685AD'
      };
      return typeColors[type] || '#A8A77A';
    }
  }
};
</script>

<style scoped>
.title {
  font-family: 'Roboto', sans-serif;
  font-size: 32px;
  color: solid white;
  text-align: center;
  margin-bottom: 20px;
}


.pokemon-list-container {
  background-color: rgba(0, 0, 0, 0.8);
  padding: 20px;
  border-radius: 10px;
  max-height: 400px;
  overflow-y: auto;
}

.pokemon-list {
  list-style-type: none;
  padding: 0;
  margin: 0;
}

.pokemon-list li {
  display: flex;
  align-items: center;
  justify-content: space-between;
  margin-bottom: 10px;
  cursor: pointer;
  background-color: rgba(255, 255, 255, 0.1);
  padding: 10px;
  border-radius: 5px;
  transition: background-color 0.3s ease;
}

.pokemon-list li:hover {
  background-color: rgba(255, 255, 255, 0.3);
}

img {
  margin-right: 10px;
}
</style>
