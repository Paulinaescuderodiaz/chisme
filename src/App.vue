<template>
  <div class="app" :class="{ 'dark': modoOscuro }">

    <!-- NAV -->
    <nav class="nav">
      <h1 class="logo">Chismecito 💗</h1>
      <div class="nav__right">
        <ul>
          <li>Inicio</li>
          <li>Explorar</li>
          <li>Perfil</li>
        </ul>
        <button class="btn-dark" @click="modoOscuro = !modoOscuro">
          {{ modoOscuro ? '☀️' : '🌙' }}
        </button>
      </div>
    </nav>

    <!-- SETUP DE NOMBRE (si no tiene nombre aún) -->
    <div v-if="!nombreConfirmado" class="nombre-box">
      <p class="nombre-box__titulo">¿Cómo te llamas? 👀</p>
      <div class="nombre-box__fila">
        <input
          v-model="nombreInput"
          placeholder="Tu nombre o apodo..."
          @keydown.enter="confirmarNombre"
          maxlength="30"
        />
        <button class="btn-publicar" @click="confirmarNombre" :disabled="!nombreInput.trim()">
          Entrar 🚀
        </button>
      </div>
    </div>

    <!-- POST BOX (solo si tiene nombre) -->
    <div v-else class="post-box">
      <div class="post-box__header">
        <div class="post-box__avatar">{{ inicial }}</div>
        <textarea
          v-model="nuevoChisme"
          :placeholder="`Cuenta el chisme, ${nombreUsuario}... 👀`"
          @keydown.ctrl.enter="agregarChisme"
        ></textarea>
      </div>
      <div class="post-box__footer">
        <div class="post-box__footer-left">
          <span class="nombre-tag">{{ nombreUsuario }}</span>
          <button class="btn-cambiar" @click="nombreConfirmado = false">Cambiar nombre</button>
        </div>
        <div>
          <span class="post-box__hint">Ctrl + Enter</span>
          <button
            class="btn-publicar"
            :disabled="!nuevoChisme.trim()"
            @click="agregarChisme"
          >
            Publicar 🚀
          </button>
        </div>
      </div>
    </div>

    <!-- CONTADOR -->
    <div class="contador" v-if="chismes.length > 0">
      {{ chismes.length }} chisme{{ chismes.length !== 1 ? 's' : '' }} publicado{{ chismes.length !== 1 ? 's' : '' }}
    </div>

    <!-- LISTA -->
    <transition-group name="chisme" tag="div" class="chismes">
      <div
        v-for="(chisme, index) in chismes"
        :key="chisme.id"
        class="chisme-card"
      >
        <div class="chisme-card__header">
          <div class="chisme-card__avatar">{{ chisme.inicial }}</div>
          <div class="chisme-card__meta">
            <span class="chisme-card__user">{{ chisme.autor }}</span>
            <span class="chisme-card__time">{{ chisme.hora }}</span>
          </div>
          <button class="chisme-card__delete" @click="eliminarChisme(index)">✕</button>
        </div>

        <p class="chisme-card__texto">{{ chisme.texto }}</p>

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

    <!-- VACÍO -->
    <div v-if="chismes.length === 0 && nombreConfirmado" class="empty">
      <p>😶 Aún no hay chismes...</p>
      <p>¡Sé el primero en contar algo!</p>
    </div>

  </div>
</template>

<script setup lang="ts">
import { ref, computed } from 'vue'

interface Chisme {
  id: number
  texto: string
  likes: number
  liked: boolean
  hora: string
  autor: string
  inicial: string
}

const modoOscuro      = ref(false)
const nombreInput     = ref('')
const nombreUsuario   = ref('')
const nombreConfirmado = ref(false)
const nuevoChisme     = ref('')
const chismes         = ref<Chisme[]>([])
let nextId = 0

const inicial = computed(() =>
  nombreUsuario.value.charAt(0).toUpperCase()
)

function confirmarNombre() {
  if (!nombreInput.value.trim()) return
  nombreUsuario.value    = nombreInput.value.trim()
  nombreConfirmado.value = true
}

function agregarChisme() {
  if (!nuevoChisme.value.trim()) return
  const hora = new Date().toLocaleTimeString('es-CL', { hour: '2-digit', minute: '2-digit' })
  chismes.value.unshift({
    id: nextId++,
    texto: nuevoChisme.value.trim(),
    likes: 0,
    liked: false,
    hora,
    autor: nombreUsuario.value,
    inicial: inicial.value
  })
  nuevoChisme.value = ''
}

function toggleLike(index: number) {
  const c = chismes.value[index]
  c.liked ? (c.likes--, c.liked = false) : (c.likes++, c.liked = true)
}

function eliminarChisme(index: number) {
  chismes.value.splice(index, 1)
}
</script>

<style scoped>
/* ── VARIABLES ── */
.app {
  --bg:       #f5e0eb;
  --surface:  #fdf0f5;
  --border:   #f0b8d0;
  --text:     #333333;
  --muted:    #999999;
  --primary:  #ff6b9d;
  --primary2: #ff8fab;
  --tag:      #ff8fab;

  max-width: 620px;
  margin: 0 auto;
  padding: 1rem;
  font-family: 'Segoe UI', sans-serif;
  background: var(--bg);
  min-height: 100vh;
  transition: background 0.3s, color 0.3s;
  color: var(--text);
}

/* MODO OSCURO */
.app.dark {
  --bg:      #1a1a2e;
  --surface: #16213e;
  --border:  #2a2a4a;
  --text:    #f0f0f0;
  --muted:   #888888;
  --primary: #c084fc;
  --primary2: #a855f7;
  --tag:     #a855f7;
}

/* NAV */
.nav {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 1rem 1.5rem;
  background: linear-gradient(135deg, var(--primary), var(--primary2));
  border-radius: 16px;
  margin-bottom: 1.5rem;
  box-shadow: 0 4px 20px rgba(0,0,0,0.15);
}
.logo { color: white; font-size: 1.4rem; font-weight: 700; }
.nav__right { display: flex; align-items: center; gap: 1.5rem; }
ul { list-style: none; display: flex; gap: 1.5rem; color: white; cursor: pointer; font-size: 0.9rem; }
li:hover { opacity: 0.75; }

.btn-dark {
  background: rgba(255,255,255,0.2);
  border: none;
  border-radius: 50%;
  width: 2rem; height: 2rem;
  cursor: pointer;
  font-size: 1rem;
  transition: background 0.2s;
}
.btn-dark:hover { background: rgba(255,255,255,0.35); }

/* NOMBRE BOX */
.nombre-box {
  background: var(--surface);
  border-radius: 16px;
  padding: 2rem;
  margin-bottom: 1.5rem;
  border: 2px solid var(--border);
  text-align: center;
  box-shadow: 0 2px 16px rgba(0,0,0,0.07);
}
.nombre-box__titulo {
  font-size: 1.1rem;
  font-weight: 700;
  color: var(--primary);
  margin-bottom: 1rem;
}
.nombre-box__fila {
  display: flex;
  gap: 0.75rem;
  max-width: 380px;
  margin: 0 auto;
}
input {
  flex: 1;
  padding: 0.6rem 1rem;
  border: 2px solid var(--border);
  border-radius: 20px;
  font-size: 0.9rem;
  outline: none;
  background: var(--bg);
  color: var(--text);
  transition: border-color 0.2s;
}
input:focus { border-color: var(--primary); }

/* POST BOX */
.post-box {
  background: var(--surface);
  border-radius: 16px;
  padding: 1.25rem;
  margin-bottom: 1rem;
  box-shadow: 0 2px 16px rgba(0,0,0,0.07);
  border: 2px solid var(--border);
}
.post-box__header {
  display: flex;
  gap: 0.75rem;
  margin-bottom: 0.75rem;
}
.post-box__avatar {
  width: 40px; height: 40px;
  border-radius: 50%;
  background: linear-gradient(135deg, var(--primary), var(--primary2));
  color: white;
  font-weight: 700;
  font-size: 1.1rem;
  display: flex; align-items: center; justify-content: center;
  flex-shrink: 0;
}
textarea {
  flex: 1;
  border: none;
  outline: none;
  resize: none;
  font-size: 0.95rem;
  height: 80px;
  font-family: inherit;
  color: var(--text);
  background: transparent;
}
textarea::placeholder { color: var(--muted); }

.post-box__footer {
  display: flex;
  justify-content: space-between;
  align-items: center;
  border-top: 1px solid var(--border);
  padding-top: 0.75rem;
  gap: 0.5rem;
  flex-wrap: wrap;
}
.post-box__footer-left { display: flex; align-items: center; gap: 0.75rem; }
.post-box__hint { font-size: 0.72rem; color: var(--muted); margin-right: 0.5rem; }

.nombre-tag {
  font-size: 0.8rem;
  font-weight: 700;
  color: var(--primary);
  background: color-mix(in srgb, var(--primary) 10%, transparent);
  padding: 0.2rem 0.7rem;
  border-radius: 20px;
}
.btn-cambiar {
  background: none;
  border: none;
  color: var(--muted);
  font-size: 0.72rem;
  cursor: pointer;
  text-decoration: underline;
}
.btn-cambiar:hover { color: var(--primary); }

.btn-publicar {
  padding: 0.5rem 1.25rem;
  background: linear-gradient(135deg, var(--primary), var(--primary2));
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
  color: var(--primary);
  margin-bottom: 0.75rem;
  font-weight: 600;
}

/* CARDS */
.chisme-card {
  background: var(--surface);
  border-radius: 16px;
  padding: 1.25rem;
  margin-bottom: 1rem;
  box-shadow: 0 2px 16px rgba(0,0,0,0.07);
  border: 1px solid var(--border);
  transition: transform 0.2s, box-shadow 0.2s;
}
.chisme-card:hover {
  transform: translateY(-2px);
  box-shadow: 0 6px 24px rgba(0,0,0,0.12);
}
.chisme-card__header {
  display: flex;
  align-items: center;
  gap: 0.75rem;
  margin-bottom: 0.75rem;
}
.chisme-card__avatar {
  width: 40px; height: 40px;
  border-radius: 50%;
  background: linear-gradient(135deg, var(--primary), var(--primary2));
  color: white;
  font-weight: 700;
  font-size: 1rem;
  display: flex; align-items: center; justify-content: center;
  flex-shrink: 0;
}
.chisme-card__meta { display: flex; flex-direction: column; flex: 1; }
.chisme-card__user { font-weight: 700; font-size: 0.88rem; color: var(--text); }
.chisme-card__time { font-size: 0.72rem; color: var(--muted); }
.chisme-card__delete {
  background: none; border: none;
  color: var(--muted); cursor: pointer; font-size: 0.85rem;
  transition: color 0.2s;
}
.chisme-card__delete:hover { color: var(--primary); }
.chisme-card__texto {
  font-size: 0.95rem;
  color: var(--text);
  line-height: 1.6;
  margin-bottom: 1rem;
}
.chisme-card__footer {
  display: flex;
  align-items: center;
  justify-content: space-between;
  border-top: 1px solid var(--border);
  padding-top: 0.75rem;
}
.btn-like {
  display: flex; align-items: center; gap: 0.4rem;
  background: none;
  border: 1px solid var(--border);
  border-radius: 20px;
  padding: 0.3rem 0.9rem;
  cursor: pointer;
  font-size: 0.85rem;
  color: var(--muted);
  transition: all 0.2s;
}
.btn-like:hover { border-color: var(--primary); color: var(--primary); }
.btn-like--activo {
  background: color-mix(in srgb, var(--primary) 10%, transparent);
  border-color: var(--primary);
  color: var(--primary);
  font-weight: 600;
}
.chisme-card__tag { font-size: 0.72rem; color: var(--tag); font-weight: 600; }

/* EMPTY */
.empty {
  text-align: center;
  padding: 3rem 1rem;
  color: var(--muted);
  font-size: 0.95rem;
  line-height: 2;
}

/* ANIMACIONES */
.chisme-enter-active { transition: all 0.35s ease; }
.chisme-enter-from { opacity: 0; transform: translateY(-10px); }
.chisme-leave-active { transition: all 0.25s ease; }
.chisme-leave-to { opacity: 0; transform: scale(0.95); }
</style>
