<template>
  <div class="player-boards-container">
    <!-- Web controls hidden on print -->
    <div class="print-controls no-print">
      <div class="header-content">
        <button @click="$emit('go-home')" class="btn-back">
          <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
            <line x1="19" y1="12" x2="5" y2="12"></line>
            <polyline points="12 19 5 12 12 5"></polyline>
          </svg>
          Voltar ao Início
        </button>
        <h1>Tabuleiros de Jogador</h1>
        <p class="subtitle">Tabuleiros A4 para Impressão das Profissões (SBPC 2026)</p>
      </div>
      <div class="info-and-actions">
        <div class="info-badge">
          <span class="count">{{ profissoes.length }}</span> tabuleiros disponíveis
        </div>
        <button @click="printBoards" class="btn-print">
          <svg xmlns="http://www.w3.org/2000/svg" width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
            <polyline points="6 9 6 2 18 2 18 9"></polyline>
            <path d="M6 18H4a2 2 0 0 1-2-2v-5a2 2 0 0 1 2-2h16a2 2 0 0 1 2 2v5a2 2 0 0 1-2 2h-2"></path>
            <rect x="6" y="14" width="12" height="8"></rect>
          </svg>
          Imprimir Todos (A4)
        </button>
      </div>
    </div>

    <!-- Boards List -->
    <div class="boards-list">
      <div 
        v-for="prof in profissoes" 
        :key="prof.id" 
        class="board-page"
        :class="getProfessionTheme(prof.colecoes_permitidas)"
      >
        <!-- A4 Page Frame -->
        <div class="board-inner">
          
          <!-- TOP AREA (20% of Height) -->
          <header class="board-top">
            <div class="top-info">
              <span class="brand-tag">SBPC 2026 • Jogo de Tabuleiro</span>
              <h2 class="prof-name">{{ prof.profissao }}</h2>
              <p class="prof-motto">{{ prof.lema }}</p>
            </div>
            <!-- Avatar Empty Space (Image Placeholder) -->
            <div class="avatar-box">
              <div class="avatar-placeholder">
                <span class="avatar-label">Espaço do Jogador</span>
                <span class="avatar-desc no-print">{{ prof.descricao_imagem }}</span>
              </div>
            </div>
          </header>

          <!-- CENTRAL AREA (60% of Height) -->
          <main class="board-center">
            <div class="slots-container">
              
              <!-- Dice Slots (dados_base) -->
              <div class="slots-group">
                <h3 class="group-title">Dados Físicos Base ({{ prof.atributos.dados_base }})</h3>
                <div class="slots-list">
                  <div 
                    v-for="d in prof.atributos.dados_base" 
                    :key="'dado-' + d" 
                    class="slot-item slot-dado"
                  >
                    <div class="slot-dashed-border">
                      <div class="slot-icon">🎲</div>
                      <span class="slot-name">Dado Físico</span>
                    </div>
                  </div>
                </div>
              </div>

              <!-- Card Slots (limite_cartas) -->
              <div class="slots-group">
                <h3 class="group-title">Slots de Cartas Ativas ({{ prof.atributos.limite_cartas }})</h3>
                <div class="slots-list">
                  <div 
                    v-for="c in prof.atributos.limite_cartas" 
                    :key="'carta-' + c" 
                    class="slot-item slot-carta"
                  >
                    <div class="slot-dashed-border">
                      <div class="slot-icon">🃏</div>
                      <span class="slot-name">Carta de Recurso</span>
                      <span class="slot-meta">Coleções Permitidas: {{ prof.colecoes_permitidas.join(', ') }}</span>
                    </div>
                  </div>
                </div>
              </div>

            </div>
          </main>

          <!-- FOOTER AREA (20% of Height) -->
          <footer class="board-footer">
            <div class="cost-table-wrapper">
              <h4 class="table-title">Tabela de Custo do Dado Comunitário</h4>
              <table class="cost-table">
                <thead>
                  <tr>
                    <th>Dado Comunitário</th>
                    <th>1º Dado</th>
                    <th>2º Dado</th>
                    <th>3º Dado</th>
                    <th>4º Dado</th>
                  </tr>
                </thead>
                <tbody>
                  <tr>
                    <td>Custo de Aquisição</td>
                    <td class="cost-value">2 pts</td>
                    <td class="cost-value">4 pts</td>
                    <td class="cost-value">8 pts</td>
                    <td class="cost-value">16 pts</td>
                  </tr>
                </tbody>
              </table>
            </div>
          </footer>

        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import profissoes from './profissoes.json';

const getProfessionTheme = (colecoes) => {
  // Maps colors to collections to style each player board matching its domain
  if (colecoes.includes('Histopatológica')) return 'theme-histopatologica';
  if (colecoes.includes('Microbiológica')) return 'theme-microbiologica';
  if (colecoes.includes('Zoológica') && colecoes.includes('Botânica')) return 'theme-biologo'; // Biólogo has Botânica + Zoológica
  if (colecoes.includes('Zoológica')) return 'theme-zoologica';
  if (colecoes.includes('Botânica')) return 'theme-botanica';
  if (colecoes.includes('Arqueopaleontológica')) return 'theme-arqueopaleontologica';
  return 'theme-epidemiologista'; // Epidemiologista (Any collection / Slate)
};

const printBoards = () => {
  window.print();
};
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Cinzel:wght@600;700;900&family=Outfit:wght@300;400;500;600;700&display=swap');

/* Style Constants & Theme Colors */
.theme-histopatologica {
  --primary-color: #d81b60;
  --primary-light: #fce4ec;
  --primary-dark: #880e4f;
  --bg-gradient: linear-gradient(135deg, #fff5f7, #fdf0f4);
}

.theme-microbiologica {
  --primary-color: #00897b;
  --primary-light: #e0f2f1;
  --primary-dark: #004d40;
  --bg-gradient: linear-gradient(135deg, #f0faf9, #e6f6f4);
}

.theme-zoologica {
  --primary-color: #e65100;
  --primary-light: #fff3e0;
  --primary-dark: #bf360c;
  --bg-gradient: linear-gradient(135deg, #fffaf4, #fff5e6);
}

.theme-botanica {
  --primary-color: #2e7d32;
  --primary-light: #e8f5e9;
  --primary-dark: #1b5e20;
  --bg-gradient: linear-gradient(135deg, #f4faf4, #eaf6ea);
}

.theme-biologo {
  --primary-color: #558b2f; /* Lime/Green split */
  --primary-light: #f1f8e9;
  --primary-dark: #33691e;
  --bg-gradient: linear-gradient(135deg, #f9fbf7, #f1f7eb);
}

.theme-arqueopaleontologica {
  --primary-color: #4e342e;
  --primary-light: #efebe9;
  --primary-dark: #3e2723;
  --bg-gradient: linear-gradient(135deg, #f7f5f4, #f0ecea);
}

.theme-epidemiologista {
  --primary-color: #455a64; /* Slate blue */
  --primary-light: #eceff1;
  --primary-dark: #263238;
  --bg-gradient: linear-gradient(135deg, #f5f7f8, #ebf0f2);
}

/* Page/Layout setup */
.player-boards-container {
  font-family: 'Outfit', sans-serif;
  background-color: #0f1115;
  color: #f1f5f9;
  min-height: 100vh;
  padding: 30px 20px;
  box-sizing: border-box;
}

.print-controls {
  max-width: 900px;
  margin: 0 auto 40px auto;
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

.btn-back {
  background: transparent;
  color: #94a3b8;
  border: 1px solid rgba(255, 255, 255, 0.1);
  border-radius: 8px;
  padding: 8px 16px;
  font-weight: 600;
  font-size: 13px;
  cursor: pointer;
  display: inline-flex;
  align-items: center;
  gap: 8px;
  transition: all 0.2s ease;
}

.btn-back:hover {
  background: rgba(255, 255, 255, 0.05);
  color: #f8fafc;
}

.header-content h1 {
  font-family: 'Cinzel', serif;
  font-size: 24px;
  margin: 0 0 6px 0;
  background: linear-gradient(90deg, #38bdf8, #a855f7);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  font-weight: 800;
}

.header-content .subtitle {
  color: #94a3b8;
  font-size: 14px;
  margin: 0;
}

.info-and-actions {
  display: flex;
  align-items: center;
  gap: 12px;
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
  transition: all 0.2s ease;
  box-shadow: 0 4px 14px rgba(56, 189, 248, 0.3);
}

.btn-print:hover {
  transform: translateY(-2px);
  box-shadow: 0 6px 20px rgba(56, 189, 248, 0.4);
}

/* Boards List container */
.boards-list {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 40px;
}

/* ------------------------------------------------------------- */
/* RIGID A4 PLAYER BOARD (210mm x 297mm)                         */
/* ------------------------------------------------------------- */
.board-page {
  width: 210mm;
  height: 297mm;
  box-sizing: border-box;
  background-color: #ffffff;
  color: #1e293b;
  position: relative;
  overflow: hidden;
  padding: 10mm;
  box-shadow: 0 15px 35px rgba(0, 0, 0, 0.4);
  
  /* Print setup */
  page-break-after: always;
  break-after: page;
  -webkit-print-color-adjust: exact;
  print-color-adjust: exact;
}

.board-page:last-child {
  page-break-after: avoid;
  break-after: avoid;
}

/* Inner frame */
.board-inner {
  width: 100%;
  height: 100%;
  box-sizing: border-box;
  border: 1mm solid var(--primary-color, #1e293b);
  border-radius: 4mm;
  background: var(--bg-gradient, #ffffff);
  padding: 8mm;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
}

/* ========================================== */
/* 1. TOP AREA (20% height of A4)             */
/* ========================================== */
.board-top {
  height: 20%;
  box-sizing: border-box;
  border-bottom: 0.5mm solid rgba(0,0,0,0.1);
  display: flex;
  justify-content: space-between;
  align-items: flex-start;
  padding-bottom: 4mm;
}

.top-info {
  flex-grow: 1;
  padding-right: 6mm;
}

.brand-tag {
  font-size: 8pt;
  font-weight: 700;
  text-transform: uppercase;
  color: var(--primary-color);
  letter-spacing: 1.5px;
  display: block;
  margin-bottom: 2mm;
}

.prof-name {
  font-family: 'Cinzel', serif;
  font-size: 24pt;
  font-weight: 900;
  color: var(--primary-dark);
  margin: 0 0 2mm 0;
  letter-spacing: 0.5px;
}

.prof-motto {
  font-size: 11pt;
  font-style: italic;
  color: #475569;
  margin: 0;
  line-height: 1.4;
  border-left: 0.8mm solid var(--primary-color);
  padding-left: 3mm;
}

/* Avatar Placeholder Box */
.avatar-box {
  width: 50mm;
  height: 35mm;
  box-sizing: border-box;
  flex-shrink: 0;
}

.avatar-placeholder {
  width: 100%;
  height: 100%;
  box-sizing: border-box;
  border: 0.5mm dashed var(--primary-color);
  border-radius: 2mm;
  background-color: rgba(255, 255, 255, 0.65);
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  padding: 3mm;
  text-align: center;
}

.avatar-label {
  font-family: 'Cinzel', serif;
  font-size: 9pt;
  font-weight: 700;
  color: var(--primary-dark);
  margin-bottom: 1.5mm;
  text-transform: uppercase;
  letter-spacing: 0.5px;
}

.avatar-desc {
  font-size: 7.5pt;
  line-height: 1.3;
  color: #64748b;
}

/* ========================================== */
/* 2. CENTRAL AREA (60% height of A4)         */
/* ========================================== */
.board-center {
  height: 60%;
  box-sizing: border-box;
  display: flex;
  flex-direction: column;
  justify-content: center;
  padding: 6mm 0;
}

.slots-container {
  display: flex;
  flex-direction: column;
  gap: 8mm;
}

.slots-group {
  display: flex;
  flex-direction: column;
  gap: 3mm;
}

.group-title {
  font-family: 'Cinzel', serif;
  font-size: 11pt;
  font-weight: 700;
  color: var(--primary-dark);
  margin: 0;
  text-transform: uppercase;
  letter-spacing: 0.8px;
}

.slots-list {
  display: flex;
  flex-wrap: wrap;
  gap: 6mm;
}

/* General Slot */
.slot-item {
  box-sizing: border-box;
  display: flex;
  align-items: center;
  justify-content: center;
}

.slot-dashed-border {
  width: 100%;
  height: 100%;
  box-sizing: border-box;
  border: 0.8mm dotted var(--primary-color);
  border-radius: 3mm;
  background-color: rgba(255, 255, 255, 0.4);
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  padding: 4mm;
  text-align: center;
}

/* Dice Slot: 35mm x 35mm */
.slot-dado {
  width: 35mm;
  height: 35mm;
}

.slot-dado .slot-icon {
  font-size: 26pt;
  margin-bottom: 2mm;
}

.slot-dado .slot-name {
  font-size: 9pt;
  font-weight: 700;
  color: #334155;
  text-transform: uppercase;
  letter-spacing: 0.5px;
}

/* Card Slot: exact size 59mm x 85mm */
.slot-carta {
  width: 59mm;
  height: 85mm;
}

.slot-carta .slot-icon {
  font-size: 30pt;
  margin-bottom: 3mm;
}

.slot-carta .slot-name {
  font-size: 9pt;
  font-weight: 700;
  color: #334155;
  text-transform: uppercase;
  letter-spacing: 0.5px;
  margin-bottom: 1.5mm;
}

.slot-meta {
  font-size: 6.5pt;
  color: #64748b;
  line-height: 1.3;
}

/* ========================================== */
/* 3. FOOTER AREA (20% height of A4)          */
/* ========================================== */
.board-footer {
  height: 20%;
  box-sizing: border-box;
  border-top: 0.5mm solid rgba(0,0,0,0.1);
  display: flex;
  align-items: flex-end;
  padding-top: 4mm;
}

.cost-table-wrapper {
  width: 100%;
}

.table-title {
  font-family: 'Cinzel', serif;
  font-size: 10pt;
  font-weight: 700;
  color: var(--primary-dark);
  margin: 0 0 3mm 0;
  text-transform: uppercase;
  letter-spacing: 0.8px;
}

.cost-table {
  width: 100%;
  border-collapse: collapse;
  box-shadow: 0 1mm 2mm rgba(0,0,0,0.02);
  border-radius: 2mm;
  overflow: hidden;
}

.cost-table th, 
.cost-table td {
  padding: 3mm 4mm;
  font-size: 9pt;
  text-align: center;
}

.cost-table th {
  background-color: var(--primary-dark);
  color: #ffffff;
  font-weight: 700;
  text-transform: uppercase;
  font-size: 8pt;
  letter-spacing: 0.5px;
}

.cost-table td {
  background-color: rgba(255,255,255,0.7);
  border: 0.25mm solid rgba(0,0,0,0.08);
  font-weight: 500;
  color: #334155;
}

.cost-table td:first-child {
  font-weight: 700;
  text-align: left;
  background-color: rgba(255, 255, 255, 0.9);
  color: var(--primary-dark);
}

.cost-value {
  font-size: 10pt !important;
  font-weight: 700 !important;
  color: var(--primary-color) !important;
}

/* ------------------------------------------------------------- */
/* PRINT STYLESHEET                                              */
/* ------------------------------------------------------------- */
@media print {
  html, body {
    background: #ffffff !important;
    color: #000000 !important;
    width: 210mm;
    height: 297mm;
    margin: 0 !important;
    padding: 0 !important;
  }
  
  .player-boards-container {
    background: transparent !important;
    padding: 0 !important;
    margin: 0 !important;
  }
  
  .no-print {
    display: none !important;
    height: 0 !important;
  }
  
  .board-page {
    box-shadow: none !important;
    margin: 0 !important;
    border: none !important;
    
    /* Force exact A4 pages print */
    page-break-after: always !important;
    break-after: page !important;
    page-break-inside: avoid !important;
    break-inside: avoid !important;
  }
  
  .board-page:last-child {
    page-break-after: avoid !important;
    break-after: avoid !important;
  }

  .board-inner {
    border-color: var(--primary-color) !important;
    background: var(--bg-gradient) !important;
  }
  
  .slot-dashed-border {
    background-color: rgba(255, 255, 255, 0.4) !important;
    border-color: var(--primary-color) !important;
  }

  .avatar-placeholder {
    background-color: rgba(255, 255, 255, 0.65) !important;
    border-color: var(--primary-color) !important;
  }
  
  .cost-table td {
    background-color: rgba(255,255,255,0.7) !important;
  }
  
  .cost-table td:first-child {
    background-color: rgba(255,255,255,0.9) !important;
  }
}

@page {
  size: A4 portrait;
  margin: 0;
}
</style>
