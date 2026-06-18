<template>
  <div class="app-container">
    <!-- 1. HOME VIEW (Dashboard) -->
    <div v-if="currentView === 'home'" class="home-container">
      <header class="home-header">
        <div class="logo-fiocruz">
          <svg class="dna-logo" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
            <path d="M4.5 16.5c-1.5 1.5-2.5 3.5-2.5 5.5s3.5 2.5 5.5 2.5c2 0 4-1 5.5-2.5l9-9c1.5-1.5 2.5-3.5 2.5-5.5s-3.5-2.5-5.5-2.5c-2 0-4 1-5.5 2.5z"></path>
            <line x1="8" y1="16" x2="16" y2="8"></line>
          </svg>
          <span class="logo-text">Fiocruz SBPC 2026</span>
        </div>
        <h1>Portal de Recursos do Jogador</h1>
        <p class="subtitle">Prepare os componentes do jogo de tabuleiro das Coleções Biológicas para impressão</p>
      </header>

      <div class="dashboard-grid">
        <!-- Card 1: Card Generator -->
        <div class="dashboard-card" @click="currentView = 'cards'">
          <div class="card-glow bg-cyan"></div>
          <div class="card-icon">🃏</div>
          <h2>Gerador de Cartas</h2>
          <p class="card-desc">Visualize, edite e exporte as 25 cartas das coleções biológicas com ilustrações exclusivas 21:9 e versos alinhados para duplex.</p>
          <div class="card-badge">25 Cartas + 25 Versos</div>
          <button class="btn-action">Acessar Cartas</button>
        </div>

        <!-- Card 2: Player Boards -->
        <div class="dashboard-card" @click="currentView = 'boards'">
          <div class="card-glow bg-purple"></div>
          <div class="card-icon">📐</div>
          <h2>Fichas de Profissões</h2>
          <p class="card-desc">Visualize e imprima as 5 fichas de jogador em formato A4, com áreas demarcadas para colocação de dados e cartas ativas de cada profissão.</p>
          <div class="card-badge">5 Fichas de Profissão</div>
          <button class="btn-action">Acessar Fichas</button>
        </div>
      </div>
      
      <footer class="home-footer">
        <p>Desenvolvido para a Reunião Anual da Sociedade Brasileira para o Progresso da Ciência (SBPC 2026)</p>
      </footer>
    </div>

    <!-- 2. CARD GENERATOR VIEW -->
    <CardGenerator v-else-if="currentView === 'cards'" @go-home="currentView = 'home'" />

    <!-- 3. PLAYER BOARD VIEW -->
    <PlayerBoards v-else-if="currentView === 'boards'" @go-home="currentView = 'home'" />
  </div>
</template>

<script setup>
import { ref } from 'vue';
import CardGenerator from './components/CardGenerator.vue';
import PlayerBoards from './components/PlayerBoards.vue';

const currentView = ref('home');
</script>

<style>
/* Global App Reset */
body {
  margin: 0;
  padding: 0;
  background-color: #0d0f13;
  color: #f1f5f9;
  font-family: 'Outfit', sans-serif;
  overflow-x: hidden;
}
</style>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Cinzel:wght@600;800&family=Outfit:wght@300;400;500;600;700&display=swap');

.app-container {
  min-height: 100vh;
}

/* Home Dashboard Styling */
.home-container {
  max-width: 1200px;
  margin: 0 auto;
  padding: 60px 20px;
  display: flex;
  flex-direction: column;
  min-height: 100vh;
  justify-content: space-between;
  box-sizing: border-box;
}

.home-header {
  text-align: center;
  margin-bottom: 50px;
}

.logo-fiocruz {
  display: inline-flex;
  align-items: center;
  gap: 10px;
  background: rgba(255, 255, 255, 0.03);
  border: 1px solid rgba(255, 255, 255, 0.08);
  padding: 8px 18px;
  border-radius: 9999px;
  margin-bottom: 24px;
}

.dna-logo {
  width: 20px;
  height: 20px;
  color: #38bdf8;
}

.logo-text {
  font-size: 13px;
  font-weight: 700;
  letter-spacing: 1.5px;
  text-transform: uppercase;
  color: #cbd5e1;
}

.home-header h1 {
  font-family: 'Cinzel', serif;
  font-size: 42px;
  font-weight: 800;
  margin: 0 0 15px 0;
  background: linear-gradient(90deg, #38bdf8, #a855f7);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  letter-spacing: 0.5px;
}

.home-header .subtitle {
  font-size: 16px;
  color: #94a3b8;
  max-width: 650px;
  margin: 0 auto;
  line-height: 1.5;
}

.dashboard-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(320px, 1fr));
  gap: 40px;
  max-width: 900px;
  margin: 0 auto 50px auto;
  width: 100%;
}

/* Card Styling */
.dashboard-card {
  background: rgba(22, 28, 38, 0.6);
  border: 1px solid rgba(255, 255, 255, 0.06);
  border-radius: 24px;
  padding: 40px 30px;
  text-align: center;
  position: relative;
  overflow: hidden;
  cursor: pointer;
  display: flex;
  flex-direction: column;
  align-items: center;
  transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
  backdrop-filter: blur(12px);
}

.dashboard-card:hover {
  transform: translateY(-8px);
  border-color: rgba(255, 255, 255, 0.15);
  box-shadow: 0 20px 40px rgba(0, 0, 0, 0.4);
}

.card-icon {
  font-size: 48px;
  margin-bottom: 20px;
  background: rgba(255, 255, 255, 0.03);
  width: 80px;
  height: 80px;
  display: flex;
  align-items: center;
  justify-content: center;
  border-radius: 20px;
  border: 1px solid rgba(255, 255, 255, 0.06);
  transition: all 0.3s ease;
}

.dashboard-card:hover .card-icon {
  background: rgba(255, 255, 255, 0.08);
  border-color: rgba(255, 255, 255, 0.2);
  transform: scale(1.1);
}

.dashboard-card h2 {
  font-family: 'Cinzel', serif;
  font-size: 24px;
  margin: 0 0 12px 0;
  color: #f8fafc;
}

.card-desc {
  font-size: 14px;
  color: #94a3b8;
  line-height: 1.6;
  margin: 0 0 24px 0;
  flex-grow: 1;
}

.card-badge {
  background: rgba(255, 255, 255, 0.05);
  border: 1px solid rgba(255, 255, 255, 0.1);
  padding: 6px 16px;
  border-radius: 9999px;
  font-size: 13px;
  font-weight: 600;
  color: #e2e8f0;
  margin-bottom: 24px;
}

.btn-action {
  background: rgba(255, 255, 255, 0.04);
  color: #f8fafc;
  border: 1px solid rgba(255, 255, 255, 0.1);
  border-radius: 10px;
  padding: 12px 30px;
  font-weight: 700;
  font-size: 14px;
  cursor: pointer;
  width: 100%;
  transition: all 0.2s ease;
}

.dashboard-card:hover .btn-action {
  background: #ffffff;
  color: #0f1115;
  border-color: #ffffff;
  box-shadow: 0 4px 15px rgba(255, 255, 255, 0.25);
}

/* Card Glows */
.card-glow {
  position: absolute;
  top: -50%;
  left: -50%;
  width: 200%;
  height: 200%;
  border-radius: 40%;
  opacity: 0.03;
  pointer-events: none;
  transition: opacity 0.3s ease;
}

.dashboard-card:hover .card-glow {
  opacity: 0.08;
}

.bg-cyan {
  background: radial-gradient(circle, #38bdf8 0%, transparent 70%);
}

.bg-purple {
  background: radial-gradient(circle, #a855f7 0%, transparent 70%);
}

/* Footer */
.home-footer {
  text-align: center;
  padding-top: 40px;
  border-top: 1px solid rgba(255, 255, 255, 0.05);
}

.home-footer p {
  font-size: 12px;
  color: #64748b;
  margin: 0;
}
</style>
