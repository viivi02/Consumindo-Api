<template>
  <div id="app">
    <audio ref="backgroundMusic" :src="musicUrl" autoplay loop></audio>
    <DigimonList />
    <PokemonList />
  </div>
</template>

<script>
import DigimonList from './components/ListaDigimons.vue';
import PokemonList from './components/ListaPokemons.vue';

export default {
  components: {
    DigimonList,
    PokemonList
  },
  created() {
    this.playBackgroundMusic();
  },
  methods: {
    playBackgroundMusic() {
      const audioPlayer = new Audio(require('@/assets/Pokémon Sword & Shield OST - Gym Leader Battle (Full In-Game Version).mp3')); // Caminho para o seu arquivo de áudio
      audioPlayer.autoplay = true;
      audioPlayer.loop = true;
      audioPlayer.volume = 0.1; 
      audioPlayer.play();
    }
  }
};
</script>

<style>
#app {
  background-image: url('https://64.media.tumblr.com/b68834a68eb48cff630a50ff2be28d37/tumblr_oiq7nbHc7T1vala9ro1_r1_640.gif');
  background-size: cover;
  background-repeat: no-repeat;
  background-attachment: fixed;
  min-height: 100vh;
  min-width: 100vw;
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #b9b9b9;
  display: flex;
  justify-content: space-around;
  align-items: center;
  margin: 0;
  padding: 0;
  overflow: hidden;
}

.content {
  display: flex;
  gap: 20px;
  padding: 20px;
  background: rgba(0, 0, 0, 0.6);
  border-radius: 10px;
}

.list-container {
  width: 100%;
  height: 100%;
  margin: 20px; 
}
</style>
