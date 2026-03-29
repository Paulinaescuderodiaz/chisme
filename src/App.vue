<template>
  <div class="container">

    <!-- NAV -->
    <nav class="nav">
      <h1 class="logo">Chismecito 💗</h1>
      <ul>
        <li>Inicio</li>
        <li>Explorar</li>
        <li>Perfil</li>
      </ul>
    </nav>

    <!-- INPUT PUBLICAR -->
    <div class="post-box">
      <textarea
        v-model="nuevoChisme"
        placeholder="Cuenta el chisme... 👀"
      ></textarea>
      <button @click="agregarChisme">Publicar 🚀</button>
    </div>

    <!-- LISTA DE CHISMES -->
    <div class="chismes">
      <div
        v-for="(chisme, index) in chismes"
        :key="index"
        class="chisme-card"
      >
        <p>{{ chisme }}</p>
      </div>
    </div>

  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue'

const nuevoChisme = ref('')
const chismes = ref<string[]>([])

function agregarChisme() {
  if (nuevoChisme.value.trim()) {
    chismes.value.unshift(nuevoChisme.value)
    nuevoChisme.value = ''
  }
}
</script>

<style scoped>
.nav {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 1rem;
  background: #ff6b9d;
  border-radius: 12px;
  margin-bottom: 1rem;
}
.logo { color: white; font-size: 1.5rem; }
ul { list-style: none; display: flex; gap: 1rem; color: white; cursor: pointer; }

.post-box {
  display: flex;
  flex-direction: column;
  gap: 0.5rem;
  margin-bottom: 1.5rem;
}
textarea {
  width: 100%;
  height: 80px;
  padding: 0.75rem;
  border-radius: 8px;
  border: 2px solid #ff6b9d;
  resize: none;
}
button {
  align-self: flex-end;
  padding: 0.5rem 1.5rem;
  background: #ff6b9d;
  color: white;
  border: none;
  border-radius: 8px;
  cursor: pointer;
}

.chisme-card {
  background: #fff0f6;
  border-left: 4px solid #ff6b9d;
  padding: 1rem;
  border-radius: 8px;
  margin-bottom: 0.75rem;
}
</style>