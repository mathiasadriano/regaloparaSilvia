<script setup>
import { ref } from 'vue';

const particleContainerRef = ref(null);
const cardRef = ref(null);
let isAnimating = false;

// --- Definiciones de las formas (NUEVAS Y MEJORADAS!) ---

// Coordenadas para un corazón más denso
const heartShape = [];
for (let i = 0; i < 250; i++) { // Aumentamos los puntos del corazón
  const t = (i / 250) * 2 * Math.PI;
  const x = 14 * Math.pow(Math.sin(t), 3);
  const y = -(11 * Math.cos(t) - 4 * Math.cos(2 * t) - 2 * Math.cos(3 * t) - Math.cos(4 * t));
  heartShape.push({ x: x * 12, y: y * 12 }); // Un poco más grande
}

// Coordenadas para "ERES" con muchos más puntos
const textShape = [
  // E
  {x:-150,y:-20},{x:-150,y:-10},{x:-150,y:0},{x:-150,y:10},{x:-150,y:20},
  {x:-140,y:-20},{x:-130,y:-20},{x:-140,y:0},{x:-130,y:0},{x:-140,y:20},{x:-130,y:20},
  // R
  {x:-110,y:-20},{x:-110,y:-10},{x:-110,y:0},{x:-110,y:10},{x:-110,y:20},
  {x:-100,y:-20},{x:-90,y:-20},{x:-100,y:0},{x:-90,y:0},{x:-90,y:-10},{x:-90,y:10},{x:-90,y:20},
  // E
  {x:-70,y:-20},{x:-70,y:-10},{x:-70,y:0},{x:-70,y:10},{x:-70,y:20},
  {x:-60,y:-20},{x:-50,y:-20},{x:-60,y:0},{x:-50,y:0},{x:-60,y:20},{x:-50,y:20},
  // S
  {x:-30,y:-20},{x:-20,y:-20},{x:-30,y:-10},{x:-30,y:0},{x:-20,y:0},{x:-10,y:0},
  {x:-10,y:10},{x:-10,y:20},{x:-20,y:20},
];

// Coordenadas para "LA MEJOR" con muchos más puntos
const secondTextShape = [
  // L
  {x:-150,y:-20},{x:-150,y:-10},{x:-150,y:0},{x:-150,y:10},{x:-150,y:20},
  {x:-140,y:20},{x:-130,y:20},
  // A
  {x:-110,y:20},{x:-110,y:10},{x:-110,y:0},{x:-100,y:-10},{x:-90,y:-20},{x:-80,y:-10},
  {x:-70,y:0},{x:-70,y:10},{x:-70,y:20},{x:-90,y:0},{x:-80,y:0},
  // M
  {x:-40,y:20},{x:-40,y:10},{x:-40,y:0},{x:-40,y:-10},{x:-40,y:-20},
  {x:-30,y:-10},{x:-20,y:0},{x:-10,y:-10},{x:0,y:-20},{x:0,y:-10},{x:0,y:0},{x:0,y:10},{x:0,y:20},
  // E
  {x:20,y:-20},{x:20,y:-10},{x:20,y:0},{x:20,y:10},{x:20,y:20},
  {x:30,y:-20},{x:40,y:-20},{x:30,y:0},{x:40,y:0},{x:30,y:20},{x:40,y:20},
  // J
  {x:80,y:-20},{x:70,y:-20},{x:60,y:-20},{x:70,y:-10},{x:70,y:0},{x:70,y:10},{x:70,y:20},{x:60,y:20},{x:50,y:10},
  // O
  {x:100,y:-10},{x:100,y:0},{x:100,y:10},{x:90,y:-20},{x:110,y:-20},{x:90,y:20},{x:110,y:20},
  {x:120,y:-10},{x:120,y:0},{x:120,y:10},
  // R
  {x:140,y:-20},{x:140,y:-10},{x:140,y:0},{x:140,y:10},{x:140,y:20},
  {x:150,y:-20},{x:160,y:-20},{x:150,y:0},{x:160,y:0},{x:160,y:-10},{x:160,y:10},{x:160,y:20},
];


function triggerAnimation() {
  if (isAnimating) return;
  isAnimating = true;

  const container = particleContainerRef.value;
  const card = cardRef.value;

  // Ocultamos la tarjeta para dar espacio a la animación
  card.style.opacity = '0';

  // Limpiamos partículas de animaciones anteriores
  container.innerHTML = '';

  const particles = [];
  const totalParticles = 300; // Aumentamos para que las formas se vean más llenas

  // 1. Creamos las partículas en el centro
  for (let i = 0; i < totalParticles; i++) {
    const p = document.createElement('div');
    p.classList.add('particle');
    p.style.transform = `translate(-50%, -50%) scale(0)`;
    container.appendChild(p);
    particles.push(p);
  }

  // 2. Animamos las partículas para formar el corazón
  setTimeout(() => {
    particles.forEach((p, i) => {
      const point = heartShape[i % heartShape.length];
      p.style.transform = `translate(${point.x}px, ${point.y}px) scale(1)`;
    });
    container.classList.add('heartbeat'); // Añadimos animación de latido
  }, 100);

  // 3. Reorganizamos las partículas para formar "ERES"
  setTimeout(() => {
    container.classList.remove('heartbeat');
    particles.forEach((p, i) => {
      const point = textShape[i % textShape.length];
      // Ajustamos la posición Y para que aparezca arriba
      p.style.transform = `translate(${point.x}px, ${point.y - 60}px) scale(1)`;
    });
  }, 3000); // 3 segundos después

  // 4. Reorganizamos para formar "LA MEJOR"
  setTimeout(() => {
    particles.forEach((p, i) => {
      const point = secondTextShape[i % secondTextShape.length];
      // Ajustamos la posición Y para que aparezca debajo de "ERES"
      p.style.transform = `translate(${point.x}px, ${point.y - 20}px) scale(1)`;
    });
  }, 5000); // 2 segundos después

  // 5. Desvanecemos las partículas y mostramos la tarjeta de nuevo
  setTimeout(() => {
    particles.forEach(p => {
      p.style.opacity = '0';
    });
    card.style.opacity = '1';
    isAnimating = false; // Permitimos que la animación se repita
  }, 7500); // 2.5 segundos después
}
</script>

<template>
  <div class="main-container">
    <div class="card" ref="cardRef">
      <h1>Para mi mejor amiga</h1>
      <p>Tengo algo que decirte...</p>
      <button @click="triggerAnimation">Haz clic aquí ✨</button>
    </div>
  </div>
  <div ref="particleContainerRef" class="particle-container"></div>
</template>

<style scoped>
.main-container {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 100%;
  height: 100vh;
  position: relative;
  z-index: 10;
}

.card {
  background: #ffffff;
  padding: 40px;
  border-radius: 20px;
  box-shadow: 0 10px 30px rgba(0, 0, 0, 0.1);
  text-align: center;
  transition: opacity 0.5s ease-in-out;
}

button {
  background-color: #ef476f;
  color: white;
  border: none;
  padding: 15px 30px;
  font-size: 1.1em;
  font-weight: bold;
  border-radius: 50px;
  cursor: pointer;
  transition: transform 0.2s ease;
  margin-top: 20px;
}

button:hover {
  transform: scale(1.1);
}

.particle-container {
  position: absolute;
  top: 50%;
  left: 50%;
  width: 1px;
  height: 1px;
  z-index: 5;
}

/* Animación de latido para el corazón */
.particle-container.heartbeat {
  animation: heartbeat 1.5s infinite ease-in-out;
}
@keyframes heartbeat {
  0%, 100% { transform: scale(1); }
  50% { transform: scale(1.15); }
}

/* :global() nos permite aplicar estilos a elementos creados dinámicamente */
:global(.particle) {
  position: absolute;
  width: 4px;
  height: 4px;
  background-color: #d90429; /* Color con buen contraste */
  border-radius: 50%;
  opacity: 1;
  /* La transición es la clave para la animación suave */
  transition: transform 1.5s cubic-bezier(0.25, 1, 0.5, 1), opacity 1s ease-in-out;
}
</style>