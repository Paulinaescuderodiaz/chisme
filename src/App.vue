<template>
  <div class="app">

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
      <div class="post-box__header">
        <span class="post-box__avatar">👤</span>
        <textarea
          v-model="nuevoChisme"
          placeholder="Cuenta el chisme... 👀"
          @keydown.ctrl.enter="agregarChisme"
        ></textarea>
      </div>
      <div class="post-box__footer">
        <span class="post-box__hint">Ctrl + Enter para publicar</span>
        <button
          class="btn-publicar"
          :disabled="!nuevoChisme.trim()"
          @click="agregarChisme"
        >
          Publicar 🚀
        </button>
      </div>
    </div>

    <!-- CONTADOR -->
    <div class="contador" v-if="chismes.length > 0">
      {{ chismes.length }} chisme{{ chismes.length !== 1 ? 's' : '' }} publicado{{ chismes.length !== 1 ? 's' : '' }}
    </div>

    <!-- LISTA DE CHISMES -->
    <transition-group name="chisme" tag="div" class="chismes">
      <div
        v-for="(chisme, index) in chismes"
        :key="chisme.id"
        class="chisme-card"
      >
        <!-- Header de la card -->
        <div class="chisme-card__header">
          <div class="chisme-card__avatar">👤</div>
          <div class="chisme-card__meta">
            <span class="chisme-card__user">Anónimo</span>
            <span class="chisme-card__time">{{ chisme.hora }}</span>
          </div>
          <button class="chisme-card__delete" @click="eliminarChisme(index)">✕</button>
        </div>

        <!-- Contenido -->
        <p class="chisme-card__texto">{{ chisme.texto }}</p>

        <!-- Footer con likes -->
        <div class="chisme-card__footer">
          <button
            class="btn-like"
            :class="{ 'btn-like--activo': chisme.liked }"
            @click="toggleLike(index)"
          >
            {{ chisme.liked ? '❤️' : '🤍' }}
            <span>{{ chisme.likes }}</span>
          </button>
          <span class="chisme-card__tag">#chisme</span>
        </div>
      </div>
    </transition-group>

    <!-- ESTADO VACÍO -->
    <div v-if="chismes.length === 0" class="empty">
      <p>😶 Aún no hay chismes...</p>
      <p>¡Sé el primero en contar algo!</p>
    </div>

  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue'

interface Chisme {
  id: number
  texto: string
  likes: number
  liked: boolean
  hora: string
}

const nuevoChisme = ref('')
const chismes = ref<Chisme[]>([])
let nextId = 0

function agregarChisme() {
  if (!nuevoChisme.value.trim()) return

  const ahora = new Date()
  const hora = ahora.toLocaleTimeString('es-CL', {
    hour: '2-digit',
    minute: '2-digit'
  })

  chismes.value.unshift({
    id: nextId++,
    texto: nuevoChisme.value.trim(),
    likes: 0,
    liked: false,
    hora
  })

  nuevoChisme.value = ''
}

function toggleLike(index: number) {
  const chisme = chismes.value[index]
  if (chisme.liked) {
    chisme.likes--
    chisme.liked = false
  } else {
    chisme.likes++
    chisme.liked = true
  }
}

function eliminarChisme(index: number) {
  chismes.value.splice(index, 1)
}
</script>

<style scoped>
.app {
  max-width: 620px;
  margin: 0 auto;
  padding: 1rem;
  font-family: 'Segoe UI', sans-serif;
}

/* NAV */
.nav {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 1rem 1.5rem;
  background: linear-gradient(135deg, #ff6b9d, #ff8fab);
  border-radius: 16px;
  margin-bottom: 1.5rem;
  box-shadow: 0 4px 20px rgba(255, 107, 157, 0.4);
}
.logo { color: white; font-size: 1.4rem; font-weight: 700; }
ul {
  list-style: none;
  display: flex;
  gap: 1.5rem;
  color: white;
  cursor: pointer;
  font-size: 0.9rem;
}
li:hover { opacity: 0.75; }

/* POST BOX */
.post-box {
  background: white;
  border-radius: 16px;
  padding: 1.25rem;
  margin-bottom: 1rem;
  box-shadow: 0 2px 16px rgba(255, 107, 157, 0.15);
  border: 2px solid #ffe0ec;
}
.post-box__header {
  display: flex;
  gap: 0.75rem;
  margin-bottom: 0.75rem;
}
.post-box__avatar {
  font-size: 2rem;
  line-height: 1;
}
textarea {
  flex: 1;
  border: none;
  outline: none;
  resize: none;
  font-size: 0.95rem;
  height: 80px;
  font-family: inherit;
  color: #333;
}
textarea::placeholder { color: #bbb; }
.post-box__footer {
  display: flex;
  justify-content: space-between;
  align-items: center;
  border-top: 1px solid #ffe0ec;
  padding-top: 0.75rem;
}
.post-box__hint { font-size: 0.72rem; color: #ccc; }

.btn-publicar {
  padding: 0.5rem 1.25rem;
  background: linear-gradient(135deg, #ff6b9d, #ff8fab);
  color: white;
  border: none;
  border-radius: 20px;
  cursor: pointer;
  font-weight: 600;
  font-size: 0.85rem;
  transition: opacity 0.2s, transform 0.1s;
}
.btn-publicar:hover:not(:disabled) { opacity: 0.9; transform: scale(1.03); }
.btn-publicar:disabled { opacity: 0.4; cursor: not-allowed; }

/* CONTADOR */
.contador {
  font-size: 0.78rem;
  color: #ff6b9d;
  margin-bottom: 0.75rem;
  font-weight: 600;
  letter-spacing: 0.03em;
}

/* CARDS */
.chisme-card {
  background: white;
  border-radius: 16px;
  padding: 1.25rem;
  margin-bottom: 1rem;
  box-shadow: 0 2px 16px rgba(0,0,0,0.07);
  border: 1px solid #f5f5f5;
  transition: transform 0.2s, box-shadow 0.2s;
}
.chisme-card:hover {
  transform: translateY(-2px);
  box-shadow: 0 6px 24px rgba(255, 107, 157, 0.15);
}

.chisme-card__header {
  display: flex;
  align-items: center;
  gap: 0.75rem;
  margin-bottom: 0.75rem;
}
.chisme-card__avatar { font-size: 1.8rem; }
.chisme-card__meta {
  display: flex;
  flex-direction: column;
  flex: 1;
}
.chisme-card__user { font-weight: 700; font-size: 0.88rem; color: #333; }
.chisme-card__time { font-size: 0.72rem; color: #bbb; }

.chisme-card__delete {
  background: none;
  border: none;
  color: #ddd;
  cursor: pointer;
  font-size: 0.85rem;
  transition: color 0.2s;
}
.chisme-card__delete:hover { color: #ff6b9d; }

.chisme-card__texto {
  font-size: 0.95rem;
  color: #444;
  line-height: 1.6;
  margin-bottom: 1rem;
}

.chisme-card__footer {
  display: flex;
  align-items: center;
  justify-content: space-between;
  border-top: 1px solid #f5f5f5;
  padding-top: 0.75rem;
}

/* LIKE */
.btn-like {
  display: flex;
  align-items: center;
  gap: 0.4rem;
  background: none;
  border: 1px solid #f0f0f0;
  border-radius: 20px;
  padding: 0.3rem 0.9rem;
  cursor: pointer;
  font-size: 0.85rem;
  color: #999;
  transition: all 0.2s;
}
.btn-like:hover { border-color: #ff6b9d; color: #ff6b9d; }
.btn-like--activo {
  background: #fff0f5;
  border-color: #ff6b9d;
  color: #ff6b9d;
  font-weight: 600;
}

.chisme-card__tag {
  font-size: 0.72rem;
  color: #ff8fab;
  font-weight: 600;
}

/* EMPTY */
.empty {
  text-align: center;
  padding: 3rem 1rem;
  color: #ccc;
  font-size: 0.95rem;
  line-height: 2;
}

/* ANIMACIONES */
.chisme-enter-active { transition: all 0.35s ease; }
.chisme-enter-from { opacity: 0; transform: translateY(-10px); }
.chisme-leave-active { transition: all 0.25s ease; }
.chisme-leave-to { opacity: 0; transform: scale(0.95); }
</style>