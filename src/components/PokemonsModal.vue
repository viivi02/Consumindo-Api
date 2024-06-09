<template>
  <div v-if="visible" class="modal-overlay" @click.self="close">
    <div class="modal-content" :style="{ backgroundColor: typeColor }">
      <button class="close-button" @click="close">X</button>
      <slot></slot>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    visible: Boolean,
    typeColor: String,
  },
  methods: {
    close() {
      this.$emit('close');
    }
  }
};
</script>

<style scoped>
.modal-overlay {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: rgba(0, 0, 0, 0.7);
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 1000;
}

.modal-content {
  background: white;
  padding: 20px;
  border-radius: 10px;
  max-width: 90%;
  max-height: 90%;
  overflow-y: auto;
  position: relative;
  transition: background 0.3s ease;
}

.close-button {
  position: absolute;
  top: 10px;
  right: 10px;
  background: transparent;
  border: none;
  font-size: 20px;
  cursor: pointer;
}

.normal {
  background-color: #A8A77A;
}

.fire {
  background-color: #EE8130;
}

.water {
  background-color: #6390F0;
}

.electric {
  background-color: #F7D02C;
}

.grass {
  background-color: #7AC74C;
}

.ice {
  background-color: #96D9D6;
}

.fighting {
  background-color: #C22E28;
}

.poison {
  background-color: #A33EA1;
}

.ground {
  background-color: #E2BF65;
}

.flying {
  background-color: #A98FF3;
}

.psychic {
  background-color: #F95587;
}

.bug {
  background-color: #A6B91A;
}

.rock {
  background-color: #B6A136;
}

.ghost {
  background-color: #735797;
}

.dragon {
  background-color: #6F35FC;
}

.dark {
  background-color: #705746;
}

.steel {
  background-color: #B7B7CE;
}

.fairy {
  background-color: #D685AD;
}
</style>
