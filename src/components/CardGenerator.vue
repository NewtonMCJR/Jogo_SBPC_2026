<template>
  <div class="card-generator-container">
    <!-- Print controls visible only on screen -->
    <div class="print-controls no-print">
      <div class="header-content">
        <h1>Coleções Biológicas Fiocruz</h1>
        <p class="subtitle">Gerador de Cartas para o Jogo de Tabuleiro (SBPC 2026)</p>
      </div>
      <div class="info-and-actions">
        <div class="info-badge">
          <span class="count">{{ cartas.length }}</span> cartas prontas para impressão
        </div>
        <button @click="printCards" class="btn-print">
          <svg xmlns="http://www.w3.org/2000/svg" width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
            <polyline points="6 9 6 2 18 2 18 9"></polyline>
            <path d="M6 18H4a2 2 0 0 1-2-2v-5a2 2 0 0 1 2-2h16a2 2 0 0 1 2 2v5a2 2 0 0 1-2 2h-2"></path>
            <rect x="6" y="14" width="12" height="8"></rect>
          </svg>
          Imprimir em A4
        </button>
      </div>
    </div>

    <!-- 1. FRONTS OF CARDS (As 25 cartas de face) -->
    <div class="section-title no-print">
      <h2>Frente das Cartas</h2>
      <p>Total de 25 cartas divididas em 5 coleções biológicas</p>
    </div>

    <div class="cards-grid">
      <div 
        v-for="(carta, index) in cartas" 
        :key="index" 
        class="card card-front"
        :class="getCollectionClass(carta.colecao)"
      >
        <!-- Card Inner Border for a layered look -->
        <div class="card-inner">
          
          <!-- Card Header (Title & Subtitle) -->
          <header class="card-header">
            <div class="collection-badge-wrapper">
              <span class="collection-badge">{{ carta.colecao }}</span>
            </div>
            <h2 class="card-title">{{ carta.titulo }}</h2>
            <h3 class="card-subtitle">{{ carta.subtitulo }}</h3>
          </header>

          <!-- Illustration Area (21:9 Aspect Ratio) -->
          <div class="card-illustration">
            <div class="illustration-border">
              <img 
                :src="'/imagens/' + carta.imagem" 
                :alt="carta.descricao_ilustracao" 
                class="illustration-image" 
              />
              <!-- Overlay showing illustration text on screen hover -->
              <div class="illustration-overlay no-print">
                <p class="overlay-desc">{{ carta.descricao_ilustracao }}</p>
              </div>
            </div>
          </div>

          <!-- Action Area -->
          <div class="card-action-box">
            <span class="action-label">Ação</span>
            <p class="action-desc">{{ carta.acao }}</p>
          </div>

          <!-- Card Footer (Flavor Text) -->
          <footer class="card-footer">
            <p class="flavor-text">{{ carta.flavor_text }}</p>
          </footer>

        </div>
      </div>
    </div>

    <!-- Print Separator to push backs to a new page -->
    <div class="page-break-print"></div>

    <!-- 2. BACKS OF CARDS (Os versos das cartas) -->
    <div class="section-title backs-section-title no-print">
      <h2>Verso das Cartas</h2>
      <p>O verso de cada carta corresponde à cor e identidade visual de sua respectiva Coleção Biológica</p>
    </div>

    <div class="cards-grid backs-grid">
      <div 
        v-for="colecao in colecoes" 
        :key="colecao" 
        class="card card-back"
        :class="getCollectionClass(colecao)"
      >
        <div class="card-inner back-inner">
          <div class="back-pattern">
            <!-- Emblem / Icon based on Collection -->
            <div class="back-emblem">
              <svg v-if="colecao === 'Histopatológica'" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="icon-emblem">
                <circle cx="12" cy="12" r="10"></circle>
                <circle cx="12" cy="12" r="3"></circle>
                <circle cx="6" cy="9" r="1"></circle>
                <circle cx="17" cy="8" r="1.5"></circle>
                <circle cx="9" cy="16" r="2"></circle>
              </svg>
              <svg v-else-if="colecao === 'Microbiológica'" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="icon-emblem">
                <circle cx="12" cy="12" r="10"></circle>
                <path d="M12 6a6 6 0 0 1 6 6c0 2.2-1.8 4-4 4s-4-1.8-4-4S12 6 12 6z"></path>
                <path d="M8 8l1.5 1.5M16 8l-1.5 1.5M8 16l1.5-1.5M16 16l-1.5-1.5"></path>
              </svg>
              <svg v-else-if="colecao === 'Zoológica'" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="icon-emblem">
                <path d="M12 2v20M12 10a4 4 0 1 0 0-8 4 4 0 0 0 0 8zm0 12a4 4 0 1 0 0-8 4 4 0 0 0 0 8z"></path>
                <path d="M2 12h20M6 8l12 8M6 16l12-8"></path>
              </svg>
              <svg v-else-if="colecao === 'Botânica'" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="icon-emblem">
                <path d="M2 22C2 22 8 18 12 12C16 6 22 2 22 2C22 2 18 8 12 12C6 16 2 22 2 22Z"></path>
                <path d="M12 12l4 1M8 16l3 1M16 8l1 4M19 5l-3 1"></path>
              </svg>
              <svg v-else-if="colecao === 'Arqueopaleontológica'" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="icon-emblem">
                <path d="M17 3a5 5 0 0 0-5 5v8a5 5 0 0 0 10 0V8a5 5 0 0 0-5-5z"></path>
                <path d="M7 21a5 5 0 0 0 5-5V8a5 5 0 0 0-10 0v8a5 5 0 0 0 5 5z"></path>
                <circle cx="17" cy="8" r="2"></circle>
                <circle cx="7" cy="16" r="2"></circle>
              </svg>
            </div>
            
            <!-- Branding and Typography -->
            <div class="back-text">
              <span class="back-brand">Fiocruz</span>
              <h2 class="back-title">Coleção Biológica</h2>
              <span class="back-sub">{{ colecao }}</span>
            </div>
          </div>
          
          <div class="back-footer">
            <span>SBPC 2026 • Jogo de Tabuleiro</span>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import cartas from './cartas_fiocruz.json';

const colecoes = [
  'Histopatológica',
  'Microbiológica',
  'Zoológica',
  'Botânica',
  'Arqueopaleontológica'
];

const getCollectionClass = (colecao) => {
  const mapping = {
    'Histopatológica': 'theme-histopatologica',
    'Microbiológica': 'theme-microbiologica',
    'Zoológica': 'theme-zoologica',
    'Botânica': 'theme-botanica',
    'Arqueopaleontológica': 'theme-arqueopaleontologica'
  };
  return mapping[colecao] || '';
};

const printCards = () => {
  window.print();
};
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Cinzel:wght@600;800&family=Outfit:wght@300;400;500;600;700&display=swap');

/* Style Constants & Theme Colors */
.theme-histopatologica {
  --primary-color: #d81b60; /* Deep pink/magenta representing H&E stain */
  --primary-light: #fce4ec;
  --primary-dark: #880e4f;
  --accent-color: #ad1457;
  --bg-gradient: linear-gradient(135deg, #fff5f7, #fdf0f4);
  --border-glow: rgba(216, 27, 96, 0.2);
}

.theme-microbiologica {
  --primary-color: #00897b; /* Bio teal representing agar and petri cultures */
  --primary-light: #e0f2f1;
  --primary-dark: #004d40;
  --accent-color: #00695c;
  --bg-gradient: linear-gradient(135deg, #f0faf9, #e6f6f4);
  --border-glow: rgba(0, 137, 123, 0.2);
}

.theme-zoologica {
  --primary-color: #e65100; /* Warm orange representing fauna and vectors */
  --primary-light: #fff3e0;
  --primary-dark: #bf360c;
  --accent-color: #d84315;
  --bg-gradient: linear-gradient(135deg, #fffaf4, #fff5e6);
  --border-glow: rgba(230, 81, 0, 0.2);
}

.theme-botanica {
  --primary-color: #2e7d32; /* Forest green representing flora and natural extracts */
  --primary-light: #e8f5e9;
  --primary-dark: #1b5e20;
  --accent-color: #376f3a;
  --bg-gradient: linear-gradient(135deg, #f4faf4, #eaf6ea);
  --border-glow: rgba(46, 125, 50, 0.2);
}

.theme-arqueopaleontologica {
  --primary-color: #4e342e; /* Earthy brown representing soil, bones and archaeology */
  --primary-light: #efebe9;
  --primary-dark: #27120f;
  --accent-color: #3e2723;
  --bg-gradient: linear-gradient(135deg, #f7f5f4, #f0ecea);
  --border-glow: rgba(78, 52, 46, 0.2);
}

/* Global Container Styles for Screen View */
.card-generator-container {
  font-family: 'Outfit', sans-serif;
  background-color: #0f1115;
  color: #f1f5f9;
  min-height: 100vh;
  padding: 30px 20px;
  box-sizing: border-box;
}

.print-controls {
  max-width: 900px;
  margin: 0 auto 30px auto;
  background: rgba(22, 28, 38, 0.8);
  border: 1px solid rgba(255, 255, 255, 0.08);
  border-radius: 16px;
  padding: 24px;
  display: flex;
  flex-direction: row;
  justify-content: space-between;
  align-items: center;
  gap: 20px;
  backdrop-filter: blur(12px);
  box-shadow: 0 10px 30px rgba(0, 0, 0, 0.3);
}

.header-content h1 {
  font-family: 'Cinzel', serif;
  font-size: 24px;
  margin: 0 0 6px 0;
  background: linear-gradient(90deg, #38bdf8, #a855f7);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  font-weight: 800;
  letter-spacing: 0.5px;
}

.header-content .subtitle {
  color: #94a3b8;
  font-size: 14px;
  margin: 0;
}

.info-and-actions {
  display: flex;
  align-items: center;
  gap: 20px;
}

.info-badge {
  background-color: rgba(255, 255, 255, 0.04);
  border: 1px solid rgba(255, 255, 255, 0.1);
  padding: 8px 16px;
  border-radius: 9999px;
  font-size: 14px;
  color: #cbd5e1;
}

.info-badge .count {
  font-weight: 700;
  color: #38bdf8;
}

.btn-print {
  background: linear-gradient(135deg, #38bdf8 0%, #0284c7 100%);
  color: white;
  border: none;
  border-radius: 8px;
  padding: 10px 20px;
  font-weight: 600;
  font-size: 14px;
  cursor: pointer;
  display: flex;
  align-items: center;
  gap: 8px;
  transition: all 0.2s cubic-bezier(0.4, 0, 0.2, 1);
  box-shadow: 0 4px 14px rgba(56, 189, 248, 0.3);
}

.btn-print:hover {
  transform: translateY(-2px);
  box-shadow: 0 6px 20px rgba(56, 189, 248, 0.4);
}

.btn-print:active {
  transform: translateY(0);
}

/* Sections */
.section-title {
  max-width: 1200px;
  margin: 40px auto 20px auto;
  border-left: 4px solid #38bdf8;
  padding-left: 15px;
}

.section-title h2 {
  font-family: 'Cinzel', serif;
  font-size: 20px;
  margin: 0 0 5px 0;
  color: #f8fafc;
}

.section-title p {
  font-size: 13px;
  color: #94a3b8;
  margin: 0;
}

.backs-section-title {
  border-left-color: #a855f7;
  margin-top: 60px;
}

/* Card Grid Layout */
.cards-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, 59mm);
  gap: 20px;
  justify-content: center;
  padding: 10px;
  max-width: 1200px;
  margin: 0 auto;
}

.backs-grid {
  margin-bottom: 50px;
}

/* ------------------------------------------------------------- */
/* RIGID CARD SPECIFICATION (59mm x 85mm)                        */
/* ------------------------------------------------------------- */
.card {
  width: 59mm;
  height: 85mm;
  box-sizing: border-box;
  background-color: #ffffff;
  color: #1e293b;
  position: relative;
  overflow: hidden;
  border-radius: 4mm;
  box-shadow: 0 8px 24px rgba(0, 0, 0, 0.25);
  transition: transform 0.25s ease, box-shadow 0.25s ease;
  
  /* Print safety */
  page-break-inside: avoid;
  break-inside: avoid;
  -webkit-print-color-adjust: exact;
  print-color-adjust: exact;
  
  /* Double border effect */
  border: 3.5mm solid var(--primary-color, #1e293b);
  padding: 1.2mm;
}

.card:hover {
  transform: translateY(-6px);
  box-shadow: 0 12px 30px var(--border-glow);
}

/* Inner Frame of the Card */
.card-inner {
  width: 100%;
  height: 100%;
  box-sizing: border-box;
  border: 0.3mm solid rgba(0, 0, 0, 0.15);
  border-radius: 1.5mm;
  background: var(--bg-gradient, #ffffff);
  padding: 1.5mm;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
}

/* 1. HEADER (Title and Subtitle) */
.card-header {
  display: flex;
  flex-direction: column;
  align-items: center;
  text-align: center;
  margin-bottom: 1.5mm;
}

.collection-badge-wrapper {
  margin-bottom: 0.8mm;
}

.collection-badge {
  font-size: 4.5pt;
  font-weight: 700;
  text-transform: uppercase;
  letter-spacing: 0.6px;
  color: #ffffff;
  background-color: var(--primary-dark);
  padding: 0.4mm 1.5mm;
  border-radius: 0.6mm;
}

.card-title {
  font-family: 'Cinzel', serif;
  font-size: 7.5pt;
  font-weight: 800;
  color: var(--primary-dark);
  margin: 0;
  line-height: 1.1;
  letter-spacing: -0.1px;
}

.card-subtitle {
  font-size: 5.5pt;
  font-weight: 500;
  color: #64748b;
  margin: 0.5mm 0 0 0;
  font-style: italic;
  letter-spacing: 0.1px;
}

/* 2. ILLUSTRATION BOX WITH 21:9 IMAGE */
.card-illustration {
  width: 100%;
  margin-bottom: 2mm;
  box-sizing: border-box;
  position: relative;
}

.illustration-border {
  width: 100%;
  aspect-ratio: 21 / 9; /* Rigid 21:9 aspect ratio */
  box-sizing: border-box;
  border: 0.3mm solid var(--primary-color);
  border-radius: 1.2mm;
  background-color: #ffffff;
  display: flex;
  align-items: center;
  justify-content: center;
  overflow: hidden;
  position: relative;
}

.illustration-image {
  width: 100%;
  height: 100%;
  object-fit: cover;
  display: block;
}

/* Tooltip overlay on screen hover */
.illustration-overlay {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(15, 17, 21, 0.95);
  color: #f1f5f9;
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 2mm;
  box-sizing: border-box;
  opacity: 0;
  transition: opacity 0.25s ease;
  pointer-events: none;
}

.card:hover .illustration-overlay {
  opacity: 1;
}

.overlay-desc {
  font-size: 5.8pt;
  line-height: 1.35;
  text-align: center;
  color: #cbd5e1;
  font-weight: 400;
  margin: 0;
}

/* 3. ACTION BOX (Highlighted) */
.card-action-box {
  background: rgba(255, 255, 255, 0.9);
  border: 0.4mm solid var(--primary-color);
  border-radius: 1.5mm;
  padding: 1.8mm 1.5mm 1.2mm 1.5mm;
  margin-bottom: 1.8mm;
  position: relative;
  box-sizing: border-box;
  box-shadow: inset 0 0.5mm 1.5mm rgba(0, 0, 0, 0.03);
}

.action-label {
  position: absolute;
  top: -2.5mm;
  left: 50%;
  transform: translateX(-50%);
  font-size: 4.5pt;
  font-weight: 800;
  text-transform: uppercase;
  color: #ffffff;
  background-color: var(--primary-color);
  padding: 0.2mm 1.8mm;
  border-radius: 0.6mm;
  border: 0.2mm solid #ffffff;
  box-shadow: 0 0.5mm 1mm rgba(0, 0, 0, 0.1);
  letter-spacing: 0.4px;
}

.action-desc {
  font-size: 7.2pt;
  font-weight: 700;
  color: var(--primary-dark);
  margin: 0;
  text-align: center;
  line-height: 1.2;
}

/* 4. FOOTER (Flavor Text) */
.card-footer {
  border-top: 0.25mm solid rgba(0, 0, 0, 0.08);
  padding-top: 1mm;
  display: flex;
  align-items: center;
  justify-content: center;
  min-height: 8mm;
}

.flavor-text {
  font-size: 5.2pt;
  font-style: italic;
  text-align: center;
  color: #475569;
  margin: 0;
  line-height: 1.25;
}

/* ------------------------------------------------------------- */
/* CARD BACK SPECIFIC STYLES                                     */
/* ------------------------------------------------------------- */
.card-back {
  background: var(--primary-dark, #0f1115) !important;
  border-color: var(--primary-color) !important;
}

.back-inner {
  border: 0.3mm solid rgba(255, 255, 255, 0.15);
  background: radial-gradient(circle at center, rgba(255, 255, 255, 0.08) 0%, rgba(0, 0, 0, 0.4) 100%) !important;
  color: #ffffff;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  align-items: center;
  padding: 4mm 2mm 3mm 2mm !important;
  box-sizing: border-box;
}

.back-pattern {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  flex-grow: 1;
  width: 100%;
}

.back-emblem {
  width: 13mm;
  height: 13mm;
  border-radius: 50%;
  border: 0.4mm solid var(--primary-color);
  background: rgba(0, 0, 0, 0.3);
  display: flex;
  align-items: center;
  justify-content: center;
  margin-bottom: 5mm;
  color: #ffffff;
  box-shadow: 0 0 4mm rgba(255, 255, 255, 0.05), inset 0 0 3mm rgba(0, 0, 0, 0.6);
}

.icon-emblem {
  width: 7.5mm;
  height: 7.5mm;
  stroke: var(--primary-color);
  filter: drop-shadow(0 0 1.2mm var(--primary-color));
}

.back-text {
  text-align: center;
  display: flex;
  flex-direction: column;
  align-items: center;
}

.back-brand {
  font-size: 5.5pt;
  text-transform: uppercase;
  letter-spacing: 1.5px;
  color: var(--primary-light);
  opacity: 0.75;
  margin-bottom: 2mm;
  font-weight: 700;
}

.back-title {
  font-family: 'Cinzel', serif;
  font-size: 9pt;
  font-weight: 800;
  margin: 0 0 2mm 0;
  line-height: 1.1;
  letter-spacing: 0.5px;
  color: #ffffff;
  text-shadow: 0 1px 3px rgba(0,0,0,0.6);
}

.back-sub {
  font-size: 5.5pt;
  font-weight: 700;
  color: var(--primary-dark);
  background: #ffffff;
  padding: 0.5mm 3.5mm;
  border-radius: 9999px;
  text-transform: uppercase;
  letter-spacing: 0.6px;
  box-shadow: 0 1px 3px rgba(0,0,0,0.3);
}

.back-footer {
  border-top: 0.25mm solid rgba(255, 255, 255, 0.12);
  width: 100%;
  padding-top: 2.2mm;
  text-align: center;
}

.back-footer span {
  font-size: 4.8pt;
  letter-spacing: 0.4px;
  color: var(--primary-light);
  opacity: 0.65;
  text-transform: uppercase;
  font-weight: 500;
}

/* Page break element only used in printing */
.page-break-print {
  display: none;
}

/* ------------------------------------------------------------- */
/* PRINT STYLESHEET                                              */
/* ------------------------------------------------------------- */
@media print {
  /* Hide scrollbars and reset body background */
  html, body {
    background: #ffffff !important;
    color: #000000 !important;
    width: 210mm;
    height: 297mm;
    margin: 0 !important;
    padding: 0 !important;
  }
  
  .card-generator-container {
    background: transparent !important;
    padding: 0 !important;
    margin: 0 !important;
    width: 210mm;
  }
  
  /* Hide web controls completely */
  .no-print {
    display: none !important;
    height: 0 !important;
    margin: 0 !important;
    padding: 0 !important;
  }
  
  /* Layout the grid to fit A4 perfectly */
  .cards-grid {
    display: grid !important;
    grid-template-columns: repeat(3, 59mm) !important;
    gap: 4mm !important;
    padding: 0 !important;
    margin: 0 !important;
    justify-content: flex-start !important;
    max-width: 210mm !important;
  }
  
  /* Separator pushes backs to a clean new page when printing */
  .page-break-print {
    display: block !important;
    page-break-before: always !important;
    break-before: page !important;
  }
  
  /* Disable hover effects and shadows for print accuracy */
  .card {
    box-shadow: none !important;
    transform: none !important;
    border-color: var(--primary-color) !important;
    background: #ffffff !important;
    
    /* Strict rules to prevent half-cut cards */
    page-break-inside: avoid !important;
    break-inside: avoid !important;
  }

  .card-inner {
    background: var(--bg-gradient) !important;
  }

  .card-back {
    background: var(--primary-dark) !important;
  }

  .back-inner {
    background: radial-gradient(circle at center, rgba(255, 255, 255, 0.08) 0%, rgba(0, 0, 0, 0.4) 100%) !important;
    color: #ffffff !important;
  }
}

/* Define A4 page dimensions and reset margins */
@page {
  size: A4 portrait;
  margin: 10mm 10mm 10mm 10mm;
}
</style>
