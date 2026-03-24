<template>
  <div class="container">

    <!-- NAV -->
    <nav class="nav">
      <h1 class="logo">Chismecito 💖</h1>
      <ul>
        <li>Inicio</li>
        <li>Explorar</li>
        <li>Perfil</li>
      </ul>
    </nav>

    <!-- INPUT PUBLICAR -->
    <div class="post-box">
      <textarea v-model="nuevoChisme" placeholder="Cuenta el chisme... 👀"></textarea>
      <button @click="agregarChisme">Publicar 💅</button>
    </div>

    <!-- FEED -->
    <div class="feed">
      <div v-for="(chisme, index) in chismes" :key="index" class="post">
        <p>{{ chisme.texto }}</p>
        <div class="actions">
          <span>❤️ {{ chisme.likes }}</span>
        </div>
      </div>
    </div>

  </div>
</template>

<script>
export default {
  data() {
    return {
      nuevoChisme: "",
      chismes: [
        { texto: "Alguien dijo que estaba enfermo y subió carrete 😭", likes: 3 },
        { texto: "Mi jefe manda correos a las 11pm... ayuda", likes: 5 }
      ]
    };
  },
  methods: {
    agregarChisme() {
      if (this.nuevoChisme.trim() === "") return;

      this.chismes.unshift({
        texto: this.nuevoChisme,
        likes: 0
      });

      this.nuevoChisme = "";
    }
  }
};
</script>

body {
  margin: 0;
  font-family: 'Segoe UI', sans-serif;
  background: linear-gradient(135deg, #fff1f5, #ffe4ec);
}

/* CONTENEDOR */
.container {
  padding: 140px 20px 40px;
}

/* NAV GLASS ✨ */
.nav {
  position: fixed;
  top: 20px;
  left: 50%;
  transform: translateX(-50%);

  width: 85%;
  max-width: 900px;

  display: flex;
  justify-content: space-between;
  align-items: center;

  padding: 15px 25px;

  border-radius: 25px;

  background: rgba(255, 214, 224, 0.6);
  backdrop-filter: blur(12px);
  -webkit-backdrop-filter: blur(12px);

  box-shadow: 0 10px 30px rgba(0,0,0,0.1);

  border: 1px solid rgba(255,255,255,0.3);

  z-index: 1000;
}

/* LOGO */
.logo {
  margin: 0;
  font-weight: 700;
  color: #a64d79;
  letter-spacing: 1px;
}

/* NAV ITEMS */
.nav ul {
  list-style: none;
  display: flex;
  gap: 20px;
}

.nav li {
  cursor: pointer;
  color: #7a3e5d;
  font-weight: 500;
  position: relative;
}

/* efecto underline animado */
.nav li::after {
  content: "";
  position: absolute;
  width: 0%;
  height: 2px;
  background: #ff4d88;
  left: 0;
  bottom: -3px;
  transition: 0.3s;
}

.nav li:hover::after {
  width: 100%;
}

.nav li:hover {
  color: #ff4d88;
}

/* SECCIONES */
.section {
  margin-top: 40px;
}

.section h2 {
  color: #a64d79;
  margin-bottom: 10px;
}

/* CARDS PREMIUM 💖 */
.card {
  background: rgba(255,255,255,0.7);
  backdrop-filter: blur(10px);

  padding: 18px;
  border-radius: 20px;
  margin-top: 10px;

  box-shadow: 0 8px 25px rgba(0,0,0,0.06);

  transition: 0.3s;
}

/* hover efecto elevación */
.card:hover {
  transform: translateY(-5px) scale(1.01);
  box-shadow: 0 12px 30px rgba(0,0,0,0.1);
}

/* TEXTO */
.card p {
  margin: 0;
  color: #5c3a4a;
}
/* CAJA POST */
.post-box {
  background: rgba(255,255,255,0.7);
  backdrop-filter: blur(10px);
  padding: 15px;
  border-radius: 20px;
  margin-bottom: 30px;
  box-shadow: 0 8px 20px rgba(0,0,0,0.05);
}

.post-box textarea {
  width: 100%;
  border: none;
  resize: none;
  padding: 10px;
  border-radius: 10px;
  outline: none;
  font-family: inherit;
}

.post-box button {
  margin-top: 10px;
  background: #ff4d88;
  color: white;
  border: none;
  padding: 8px 15px;
  border-radius: 10px;
  cursor: pointer;
}

/* FEED */
.feed {
  display: flex;
  flex-direction: column;
  gap: 20px;
}

/* POST */
.post {
  background: rgba(255,255,255,0.7);
  backdrop-filter: blur(10px);
  padding: 20px;
  border-radius: 20px;
  box-shadow: 0 8px 25px rgba(0,0,0,0.06);
}

/* ACCIONES */
.actions {
  margin-top: 10px;
  font-size: 14px;
  color: #a64d79;
}
/* CENTRAR CONTENIDO */
.container {
  padding: 140px 20px 40px;
  display: flex;
  flex-direction: column;
  align-items: center;
}

/* FEED COMO INSTAGRAM */
.feed {
  width: 100%;
  max-width: 500px;
  display: flex;
  flex-direction: column;
  gap: 20px;
}

/* CAJA POST MÁS BONITA */
.post-box {
  width: 100%;
  max-width: 500px;

  background: rgba(255,255,255,0.8);
  backdrop-filter: blur(10px);

  padding: 15px;
  border-radius: 20px;
  margin-bottom: 20px;

  box-shadow: 0 8px 20px rgba(0,0,0,0.05);
}

/* TEXTAREA BONITO */
.post-box textarea {
  width: 100%;
  border: none;
  resize: none;
  padding: 12px;
  border-radius: 12px;
  outline: none;
  font-family: inherit;
  font-size: 14px;
}

/* BOTÓN MÁS INSTAGRAM */
.post-box button {
  margin-top: 10px;
  background: linear-gradient(135deg, #ff4d88, #ff85a2);
  color: white;
  border: none;
  padding: 10px 15px;
  border-radius: 12px;
  cursor: pointer;
  width: 100%;
  font-weight: bold;
}

/* POSTS MÁS PRO */
.post {
  width: 100%;

  background: rgba(255,255,255,0.8);
  backdrop-filter: blur(10px);

  padding: 18px;
  border-radius: 20px;

  box-shadow: 0 10px 25px rgba(0,0,0,0.08);

  transition: 0.3s;
}

.post:hover {
  transform: translateY(-4px);
}

/* TEXTO */
.post p {
  margin: 0;
  font-size: 15px;
  color: #4a2c3a;
}

/* LIKES */
.actions {
  margin-top: 10px;
  font-size: 14px;
  color: #ff4d88;
  font-weight: 500;
}
.nav ul {
  list-style: none;
}
