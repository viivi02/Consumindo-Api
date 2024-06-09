<template>
  <div class="digimon-list-container">
    <h1 class="title">Lista de Digimons</h1>
    <ul class="digimon-list">
      <li v-for="digimon in digimons" :key="digimon.name" @click="selectDigimon(digimon)">
        <img :src="digimon.img" :alt="digimon.name" width="50" />
        <span>{{ digimon.name }}</span>
      </li>
    </ul>

    <DigimonModal :visible="showModal" @close="closeModal">
      <div v-if="selectedDigimon">
        <h2>Detalhes do Digimon</h2>
        <img :src="selectedDigimon.img" :alt="selectedDigimon.name" width="100" />
        <p><strong>Nome:</strong> {{ selectedDigimon.name }}</p>
        <p><strong>Nível:</strong> {{ selectedDigimon.level }}</p>
      </div>
    </DigimonModal>
  </div>
</template>

<script>
import axios from 'axios';
import DigimonModal from './DigimonModal.vue';

export default {
  components: {
    DigimonModal
  },
  data() {
    return {
      digimons: [],
      selectedDigimon: null,
      showModal: false
    };
  },
  created() {
    this.fetchDigimons();
  },
  methods: {
    async fetchDigimons() {
      try {
        const response = await axios.get('https://digimon-api.vercel.app/api/digimon');
        this.digimons = response.data;
      } catch (error) {
        console.error('Erro ao buscar digimons:', error);
      }
    },
    selectDigimon(digimon) {
      this.selectedDigimon = digimon;
      this.showModal = true;
    },
    closeModal() {
      this.showModal = false;
      this.selectedDigimon = null;
    }
  }
};
</script>

<style scoped>
.digimon-list-container {
  background-color: rgba(0, 0, 0, 0.8);
  padding: 20px;
  border-radius: 10px;
  max-height: 400px;
  overflow-y: auto;
}

.title {
  font-family: 'Roboto', sans-serif;
  font-size: 32px;
  color: #ffffff;
  text-align: center;
  margin-bottom: 20px;
}

.digimon-list {
  list-style-type: none;
  padding: 0;
  margin: 0;
}

.digimon-list li {
  display: flex;
  align-items: center;
  margin-bottom: 10px;
  cursor: pointer;
  background-color: rgba(255, 255, 255, 0.1);
  padding: 10px;
  border-radius: 5px;
  transition: background-color 0.3s ease;
}

.digimon-list li:hover {
  background-color: rgba(255, 255, 255, 0.3);
}

img {
  margin-right: 10px;
}

.digimon-details {
  margin-top: 20px;
}
</style>
