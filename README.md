<!DOCTYPE html>
<html lang="pt-BR">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>TallesWeb - Construções Quilombolas</title>
<style>
/* ======== ESTILO GERAL ======== */
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: "Poppins", sans-serif;
}

body {
  background-color: #0a0a0a;
  color: #fff;
  overflow-x: hidden;
  scroll-behavior: smooth;
  transition: filter 0.3s ease;
}

/* ====== HEADER ====== */
header {
  background: linear-gradient(90deg, #ffb300, #ff9900);
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 20px;
}

header h1 {
  color: #fff;
  font-size: 2em;
  font-weight: 700;
  animation: titleFloat 3s ease-in-out infinite;
}

@keyframes titleFloat {
  0%, 100% { transform: translateY(0); }
  50% { transform: translateY(-4px); }
}

nav a {
  color: #fff;
  text-decoration: none;
  margin-left: 20px;
  font-weight: bold;
  transition: opacity 0.3s ease;
}

nav a:hover {
  opacity: 0.7;
}

/* ===== ENGENHARIA (ENGRENAGEM) ===== */
.config-btn {
  position: fixed;
  top: 20px;
  right: 25px;
  background: none;
  border: none;
  cursor: pointer;
  z-index: 999;
}

.config-btn svg {
  width: 32px;
  height: 32px;
  fill: #ffb300;
  transition: transform 0.5s ease;
}

.config-btn:hover svg {
  transform: rotate(180deg);
}

/* Painel de configurações */
.config-panel {
  position: fixed;
  top: 65px;
  right: 25px;
  background: rgba(20, 20, 20, 0.95);
  border: 1px solid #ffb300;
  border-radius: 12px;
  padding: 20px;
  width: 220px;
  box-shadow: 0 0 10px rgba(255, 179, 0, 0.3);
  display: none;
  flex-direction: column;
  z-index: 998;
  backdrop-filter: blur(10px);
  transition: opacity 0.3s ease;
}

.config-panel.show {
  display: flex;
  animation: fadeIn 0.4s ease;
}

@keyframes fadeIn {
  from { opacity: 0; transform: translateY(-10px); }
  to { opacity: 1; transform: translateY(0); }
}

.config-panel h3 {
  color: #ffb300;
  font-size: 1em;
  margin-bottom: 10px;
}

.config-panel label, 
.config-panel select {
  color: #fff;
  font-size: 0.9em;
  margin-top: 8px;
}

/* ====== SEÇÕES ====== */
section {
  position: relative;
  padding: 60px 20px;
  text-align: center;
}

.card {
  background: rgba(255, 255, 255, 0.05);
  border-radius: 20px;
  padding: 30px;
  margin: 20px auto;
  max-width: 600px;
  box-shadow: 0 0 15px rgba(255, 153, 0, 0.2);
  transition: all 0.4s ease;
}

/* ====== TÍTULOS COM ANIMAÇÃO ====== */
@keyframes textGlow {
  0%, 100% { text-shadow: 0 0 10px #ffb300; transform: scale(1); }
  50% { text-shadow: 0 0 20px #ffcc33; transform: scale(1.02); }
}

.card h2 {
  color: #ffb300;
  margin-bottom: 10px;
  display: inline-block;
  animation: textGlow 4s ease-in-out infinite;
}

/* ====== TEXTO E BOTÃO ====== */
.card p {
  color: #ccc;
  line-height: 1.6;
  transition: all 0.5s ease;
}

.extra {
  max-height: 0;
  overflow: hidden;
  opacity: 0;
  transition: all 0.6s ease;
}

.card.expanded .extra {
  max-height: 200px;
  opacity: 1;
}

button {
  background: #ffb300;
  color: #000;
  border: none;
  border-radius: 25px;
  padding: 10px 20px;
  font-weight: bold;
  margin-top: 10px;
  cursor: pointer;
  transition: all 0.3s ease;
}

button:hover {
  background: #ffa700;
  transform: scale(1.05);
}

/* ====== CANVAS FUNDO ====== */
canvas {
  position: fixed;
  top: 0;
  left: 0;
  z-index: -1;
}

/* ====== CRÉDITOS ====== */
footer {
  text-align: center;
  color: #ffb300;
  font-weight: bold;
  padding: 30px;
  animation: textGlow 4s ease-in-out infinite;
}

/* ====== MENSAGEM TEMPORÁRIA ====== */
.msg {
  position: fixed;
  bottom: 20px;
  right: 25px;
  background: rgba(255, 179, 0, 0.9);
  color: #000;
  padding: 10px 15px;
  border-radius: 12px;
  font-weight: bold;
  display: none;
  z-index: 9999;
  animation: fadeInOut 2s ease;
}

@keyframes fadeInOut {
  0%, 100% { opacity: 0; transform: translateY(10px); }
  10%, 90% { opacity: 1; transform: translateY(0); }
}
</style>
</head>
<body>

<canvas id="network"></canvas>

<header>
  <h1>TallesWeb</h1>
  <nav>
    <a href="#sobre">SOBRE</a>
    <a href="#curiosidades">CURIOSIDADES</a>
    <a href="#servicos">SERVIÇOS</a>
    <a href="#tecnologia">TECNOLOGIA</a>
    <a href="#contato">CRÉDITOS</a>
  </nav>
</header>

<!-- Botão de configurações -->
<button class="config-btn" id="configBtn" aria-label="Configurações">
  <svg viewBox="0 0 512 512">
    <path d="M487.4 315.7l-42.6-24.6c2.6-13.4 3.9-27.2 3.9-41.1s-1.3-27.7-3.9-41.1l42.6-24.6c8.7-5 12.2-15.6 8.5-25l-45.2-104c-3.7-9.4-13.3-14.8-23.2-13.1l-50.1 8.6c-10.5-8-21.8-15-33.7-21l-7.5-50.9C330.3 8 321.1 0 310.2 0h-108c-10.9 0-20.1 8-21.9 18.9l-7.5 50.9c-11.9 6-23.2 13-33.7 21l-50.1-8.6c-9.9-1.7-19.5 3.8-23.2 13.1l-45.2 104c-3.7 9.4-.2 20 8.5 25l42.6 24.6C66.3 222.3 65 236.1 65 250s1.3 27.7 3.9 41.1l-42.6 24.6c-8.7 5-12.2 15.6-8.5 25l45.2 104c3.7 9.4 13.3 14.8 23.2 13.1l50.1-8.6c10.5 8 21.8 15 33.7 21l7.5 50.9c1.8 10.9 11 18.9 21.9 18.9h108c10.9 0 20.1-8 21.9-18.9l7.5-50.9c11.9-6 23.2-13 33.7-21l50.1 8.6c9.9 1.7 19.5-3.8 23.2-13.1l45.2-104c3.7-9.4.2-20-8.5-25zM256 346c-53.5 0-97-43.5-97-97s43.5-97 97-97 97 43.5 97 97-43.5 97-97 97z"/>
  </svg>
</button>

<!-- Painel de configurações -->
<div class="config-panel" id="configPanel">
  <h3>Configurações</h3>
  <label><input type="checkbox" id="shadowToggle" checked> Ativar sombras</label>
  <label for="graphics">Qualidade gráfica:</label>
  <select id="graphics">
    <option value="240p">240p</option>
    <option value="360p">360p</option>
    <option value="480p">480p</option>
    <option value="720p" selected>720p</option>
    <option value="1080p">1080p</option>
  </select>
</div>

<!-- Mensagem -->
<div class="msg" id="msg">Configuração atualizada</div>

<!-- ====== CONTEÚDO PRINCIPAL ====== -->
<section id="sobre">
  <div class="card">
    <h2>🏠 A Sabedoria das Mãos Quilombolas</h2>
    <p>As construções quilombolas são feitas com sabedoria ancestral...</p>
    <div class="extra">Essas técnicas valorizam o meio ambiente e a coletividade.</div>
  </div>
  <div class="card">
    <h2>🌿 Arquitetura Sustentável Antes do Tempo</h2>
    <p>Muito antes de se falar em sustentabilidade...</p>
    <div class="extra">Posição solar e ventilação natural eram prioridades.</div>
  </div>
  <div class="card">
    <h2>🔥 As Cozinhas como Coração da Comunidade</h2>
    <p>A cozinha é o centro da casa, onde se compartilham histórias...</p>
    <div class="extra">O fogão a lenha simboliza união e tradição.</div>
  </div>
  <div class="card">
    <h2>🪵 Técnicas que Resistiram ao Tempo</h2>
    <p>O uso de taipa, pau-a-pique e adobe é herança cultural viva...</p>
    <div class="extra">Duráveis e ecológicas, essas práticas persistem.</div>
  </div>
  <div class="card">
    <h2>🏞️ Construções Integradas à Natureza</h2>
    <p>As moradias respeitam o relevo e os rios locais...</p>
    <div class="extra">Mostrando o profundo respeito pela terra.</div>
  </div>
  <div class="card">
    <h2>🧱 O Barro como Tecnologia Viva</h2>
    <p>O barro regula temperatura e absorve umidade...</p>
    <div class="extra">É uma tecnologia ancestral sustentável.</div>
  </div>
  <div class="card">
    <h2>🌍 Influência Africana e Mistura Cultural</h2>
    <p>As construções unem saberes africanos, indígenas e europeus...</p>
    <div class="extra">Criando um estilo único e coletivo.</div>
  </div>
  <div class="card">
    <h2>🛖 O Futuro das Construções Quilombolas</h2>
    <p>Hoje, arquitetos se inspiram nas técnicas quilombolas...</p>
    <div class="extra">Prova de que tradição e inovação caminham juntas.</div>
  </div>
  <button class="verMais">Ver Mais</button>
</section>

<footer id="contato">
  <p>Créditos — Talles Henrique Freitas de Castro © 2025</p>
</footer>

<script>
/* ===== Engrenagem ===== */
const configBtn = document.getElementById("configBtn");
const configPanel = document.getElementById("configPanel");
configBtn.addEventListener("click", () => configPanel.classList.toggle("show"));

/* ===== Shadows ===== */
const shadowToggle = document.getElementById("shadowToggle");
shadowToggle.addEventListener("change", () => {
  document.querySelectorAll(".card").forEach(card => {
    card.style.boxShadow = shadowToggle.checked ? "0 0 15px rgba(255,153,0,0.2)" : "none";
  });
});

/* ===== Gráficos ===== */
const graphics = document.getElementById("graphics");
const msg = document.getElementById("msg");
graphics.addEventListener("change", () => {
  const value = graphics.value;
  document.body.style.filter =
    value === "240p" ? "blur(1.5px)" :
    value === "360p" ? "blur(1px)" :
    value === "480p" ? "blur(0.5px)" :
    value === "720p" ? "none" :
    value === "1080p" ? "contrast(1.1) brightness(1.05)" : "none";
  msg.textContent = `Gráficos ajustados para ${value}`;
  msg.style.display = "block";
  setTimeout(() => msg.style.display = "none", 2000);
});

/* ===== Ver mais ===== */
const verMaisBtn = document.querySelector(".verMais");
verMaisBtn.addEventListener("click", () => {
  document.querySelectorAll(".card").forEach(card => card.classList.toggle("expanded"));
  verMaisBtn.textContent = verMaisBtn.textContent === "Ver Mais" ? "Ver Menos" : "Ver Mais";
});

/* ===== Canvas Rede de Pontos ===== */
const canvas = document.getElementById("network");
const ctx = canvas.getContext("2d");
let particlesArray;

function init() {
  canvas.width = window.innerWidth;
  canvas.height = window.innerHeight;
  particlesArray = [];
  const num = (canvas.width * canvas.height) / 18000;
  for (let i = 0; i < num; i++) {
    const size = Math.random() * 2 + 1;
    const x = Math.random() * canvas.width;
    const y = Math.random() * canvas.height;
    const dx = (Math.random() - 0.5) * 1.5;
    const dy = (Math.random() - 0.5) * 1.5;
    particlesArray.push({ x, y, dx, dy, size });
  }
}

function drawParticles() {
  ctx.clearRect(0, 0, canvas.width, canvas.height);
  ctx.fillStyle = "white";
  particlesArray.forEach(p => {
    ctx.beginPath();
    ctx.arc(p.x, p.y, p.size, 0, Math.PI * 2);
    ctx.fill();
  });
}

function connectParticles() {
  for (let a = 0; a < particlesArray.length; a++) {
    for (let b = a; b < particlesArray.length; b++) {
      const dx = particlesArray[a].x - particlesArray[b].x;
      const dy = particlesArray[a].y - particlesArray[b].y;
      const dist = Math.sqrt(dx * dx + dy * dy);
      if (dist < 100) {
        ctx.strokeStyle = "rgba(255,255,255,0.1)";
        ctx.lineWidth = 1;
        ctx.beginPath();
        ctx.moveTo(particlesArray[a].x, particlesArray[a].y);
        ctx.lineTo(particlesArray[b].x, particlesArray[b].y);
        ctx.stroke();
      }
    }
  }
}

function animate() {
  drawParticles();
  connectParticles();
  particlesArray.forEach(p => {
    p.x += p.dx;
    p.y += p.dy;
    if (p.x < 0 || p.x > canvas.width) p.dx = -p.dx;
    if (p.y < 0 || p.y > canvas.height) p.dy = -p.dy;
  });
  requestAnimationFrame(animate);
}

window.addEventListener("resize", init);
init();
animate();
</script>

</body>
</html>
