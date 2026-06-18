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
        <h1>Fichas de Personagem (A4)</h1>
        <p class="subtitle">Impressão gráfica frente e verso dos tabuleiros das profissões (SBPC 2026)</p>
      </div>
      <div class="info-and-actions">
        <div class="info-badge">
          <span class="count">4 páginas</span> A4 (2 Frentes / 2 Versos)
        </div>
        <button @click="printBoards" class="btn-print">
          <svg xmlns="http://www.w3.org/2000/svg" width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
            <polyline points="6 9 6 2 18 2 18 9"></polyline>
            <path d="M6 18H4a2 2 0 0 1-2-2v-5a2 2 0 0 1 2-2h16a2 2 0 0 1 2 2v5a2 2 0 0 1-2 2h-2"></path>
            <rect x="6" y="14" width="12" height="8"></rect>
          </svg>
          Imprimir Fichas Duplex
        </button>
      </div>
    </div>

    <!-- Printable A4 Sheets -->
    <div class="printable-sheets">
      <template v-for="(chunk, chunkIndex) in chunkedProfissoes" :key="'chunk-' + chunkIndex">
        
        <!-- PAGE A4: FRENTES (Fichas 1, 2, 3) -->
        <div class="page-a4 page-fronts">
          <div 
            v-for="(prof, index) in chunk" 
            :key="'front-' + prof.id + '-' + index" 
            class="ficha-row front-row"
            :class="[getProfessionTheme(prof.colecoes_permitidas), { 'row-placeholder': prof.isPlaceholder }]"
          >
            <!-- Render actual content if not a placeholder -->
            <template v-if="!prof.isPlaceholder">
              <div class="ficha-inner">
                <!-- Lado Esquerdo (40%): Nome, Lema, Tabela de Custo -->
                <div class="lado-esquerdo">
                  <div class="profissao-header">
                    <span class="brand-tag">SBPC 2026</span>
                    <h2 class="profissao-title">{{ prof.profissao }}</h2>
                    <p class="profissao-lema">{{ prof.lema }}</p>
                  </div>
                  
                  <!-- Tabela Fixa de Custo do Dado Comunitário -->
                  <div class="cost-table-container">
                    <table class="cost-table">
                      <thead>
                        <tr>
                          <th>Dado Comunitário</th>
                          <th>1º</th>
                          <th>2º</th>
                          <th>3º</th>
                          <th>4º</th>
                        </tr>
                      </thead>
                      <tbody>
                        <tr>
                          <td class="row-label">Custo</td>
                          <td class="cost-val">2</td>
                          <td class="cost-val">4</td>
                          <td class="cost-val">8</td>
                          <td class="cost-val">16</td>
                        </tr>
                      </tbody>
                    </table>
                  </div>
                </div>

                <!-- Lado Direito (60%): Slots de Dados e Cartas -->
                <div class="lado-direito">
                  <div class="slots-header">
                    <span>Área de Componentes Físicos</span>
                  </div>
                  <div class="slots-grid">
                    <!-- Slots de Dados (dados_base) -->
                    <div 
                      v-for="d in prof.atributos.dados_base" 
                      :key="'dado-' + d" 
                      class="slot-box slot-box-dado"
                    >
                      <span class="slot-icon">🎲</span>
                      <span class="slot-label">Dado</span>
                    </div>

                    <!-- Slots de Cartas (limite_cartas) -->
                    <div 
                      v-for="c in prof.atributos.limite_cartas" 
                      :key="'carta-' + c" 
                      class="slot-box slot-box-carta"
                    >
                      <span class="slot-icon">🃏</span>
                      <span class="slot-label">Carta</span>
                      <span class="slot-sub">{{ prof.colecoes_permitidas[0] }}</span>
                    </div>
                  </div>
                </div>
              </div>
            </template>
            <!-- Crop mark sutil no final da ficha -->
            <div class="crop-mark no-print"></div>
          </div>
        </div>

        <!-- PAGE A4: VERSOS (Fichas 1, 2, 3) -->
        <div class="page-a4 page-backs">
          <div 
            v-for="(prof, index) in chunk" 
            :key="'back-' + prof.id + '-' + index" 
            class="ficha-row back-row"
            :class="[getProfessionTheme(prof.colecoes_permitidas), { 'row-placeholder': prof.isPlaceholder }]"
          >
            <!-- Render actual content if not a placeholder -->
            <template v-if="!prof.isPlaceholder">
              <div class="ficha-inner back-inner">
                <div class="back-mysterious-frame">
                  <!-- Centralized Profession Title -->
                  <div class="back-header">
                    <span class="brand-tag">Coleção Biológica</span>
                    <h2 class="profissao-title">{{ prof.profissao }}</h2>
                  </div>
                  <!-- Ilustração Temática da Profissão (Verso) -->
                  <div class="back-illustration-box">
                    <img 
                      :src="'/imagens/' + getProfessionIllustration(prof.id)" 
                      :alt="prof.descricao_imagem" 
                      class="back-illustration-image"
                    />
                    <!-- Overlay de Descrição em Tela (no-print) -->
                    <div class="illustration-caption no-print">
                      <p>{{ prof.descricao_imagem }}</p>
                    </div>
                  </div>
                </div>
              </div>
            </template>
            <div class="crop-mark no-print"></div>
          </div>
        </div>

      </template>
    </div>
  </div>
</template>

<script setup>
import { computed } from 'vue';
import profissoes from './profissoes.json';

const printBoards = () => {
  window.print();
};

const getProfessionTheme = (colecoes) => {
  if (!colecoes) return '';
  if (colecoes.includes('Histopatológica')) return 'theme-histopatologica';
  if (colecoes.includes('Microbiológica')) return 'theme-microbiologica';
  if (colecoes.includes('Zoológica') && colecoes.includes('Botânica')) return 'theme-biologo';
  if (colecoes.includes('Zoológica')) return 'theme-zoologica';
  if (colecoes.includes('Botânica')) return 'theme-botanica';
  if (colecoes.includes('Arqueopaleontológica')) return 'theme-arqueopaleontologica';
  return 'theme-epidemiologista';
};

// Chunks the 5 professions into groups of 3 to fit A4 sheets (3 per sheet)
const chunkedProfissoes = computed(() => {
  const size = 3;
  const result = [];
  for (let i = 0; i < profissoes.length; i += size) {
    const chunk = profissoes.slice(i, i + size);
    // Pad the last chunk with placeholder if incomplete
    while (chunk.length < size) {
      chunk.push({ isPlaceholder: true, id: 'placeholder-' + chunk.length });
    }
    result.push(chunk);
  }
  return result;
});

const getProfessionIllustration = (id) => {
  return `ilustracao_${id}.png`;
};
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Cinzel:wght@600;800;900&family=Outfit:wght@300;400;500;600;700&display=swap');

/* Style Constants & Theme Colors */
.theme-histopatologica {
  --primary-color: #d81b60;
  --primary-light: #fce4ec;
  --primary-dark: #880e4f;
  --bg-gradient: linear-gradient(135deg, #fffcfd, #fff0f4);
}

.theme-microbiologica {
  --primary-color: #00897b;
  --primary-light: #e0f2f1;
  --primary-dark: #004d40;
  --bg-gradient: linear-gradient(135deg, #f8fcfb, #e6f6f4);
}

.theme-zoologica {
  --primary-color: #e65100;
  --primary-light: #fff3e0;
  --primary-dark: #bf360c;
  --bg-gradient: linear-gradient(135deg, #fffbf7, #fff6e8);
}

.theme-botanica {
  --primary-color: #2e7d32;
  --primary-light: #e8f5e9;
  --primary-dark: #1b5e20;
  --bg-gradient: linear-gradient(135deg, #f8fcf8, #eaf6ea);
}

.theme-biologo {
  --primary-color: #558b2f;
  --primary-light: #f1f8e9;
  --primary-dark: #33691e;
  --bg-gradient: linear-gradient(135deg, #fafcfa, #f2f7ed);
}

.theme-arqueopaleontologica {
  --primary-color: #4e342e;
  --primary-light: #efebe9;
  --primary-dark: #3e2723;
  --bg-gradient: linear-gradient(135deg, #faf9f9, #f1ecea);
}

.theme-epidemiologista {
  --primary-color: #455a64;
  --primary-light: #eceff1;
  --primary-dark: #263238;
  --bg-gradient: linear-gradient(135deg, #f7f8f9, #ebf0f2);
}

/* Page Setup for Screen View */
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
  margin-bottom: 8px;
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

/* Printable container for screen scroll view */
.printable-sheets {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 50px;
}

/* ------------------------------------------------------------- */
/* RIGID A4 LAYOUT FOR PRINT (210mm x 297mm)                     */
/* ------------------------------------------------------------- */
.page-a4 {
  width: 210mm;
  height: 297mm;
  box-sizing: border-box;
  background-color: #ffffff;
  color: #1e293b;
  position: relative;
  overflow: hidden;
  display: flex;
  flex-direction: column;
  box-shadow: 0 15px 40px rgba(0, 0, 0, 0.45);
  
  /* Print breaks */
  page-break-after: always;
  break-after: page;
  -webkit-print-color-adjust: exact;
  print-color-adjust: exact;
}

/* Card Row: Exactly 99mm high (3 rows = 297mm) */
.ficha-row {
  width: 210mm;
  height: 99mm;
  box-sizing: border-box;
  position: relative;
  display: flex;
  align-items: center;
  padding: 4mm 6mm;
}

.row-placeholder {
  background-color: #fafbfc !important;
}

/* Crop Marks / Crop lines */
.crop-mark {
  position: absolute;
  bottom: 0;
  left: 0;
  width: 100%;
  border-bottom: 0.25mm dashed #e2e8f0;
}

.ficha-row:last-child .crop-mark {
  display: none;
}

/* Inner Frame of the Card */
.ficha-inner {
  width: 100%;
  height: 100%;
  box-sizing: border-box;
  border: 1mm solid var(--primary-color, #1e293b);
  border-radius: 3mm;
  background: var(--bg-gradient, #ffffff);
  padding: 3mm;
  display: flex;
  justify-content: space-between;
  box-shadow: inset 0 0 2mm rgba(0, 0, 0, 0.02);
}

/* ============================================================= */
/* FRENTE LAYOUT (Left 40% / Right 60%)                          */
/* ============================================================= */

/* Left Column (40%) */
.lado-esquerdo {
  width: 40%;
  box-sizing: border-box;
  border-right: 0.3mm solid rgba(0, 0, 0, 0.08);
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  padding-right: 4mm;
}

.profissao-header {
  display: flex;
  flex-direction: column;
}

.brand-tag {
  font-size: 6.5pt;
  font-weight: 700;
  text-transform: uppercase;
  color: var(--primary-color);
  letter-spacing: 1.2px;
  margin-bottom: 1mm;
}

.profissao-title {
  font-family: 'Cinzel', serif;
  font-size: 16pt;
  font-weight: 900;
  color: var(--primary-dark);
  margin: 0 0 1.2mm 0;
  line-height: 1.1;
  letter-spacing: 0.2px;
}

.profissao-lema {
  font-size: 8pt;
  font-style: italic;
  color: #475569;
  line-height: 1.35;
  margin: 0;
  border-left: 0.6mm solid var(--primary-color);
  padding-left: 2.2mm;
}

/* Community Die Table */
.cost-table-container {
  width: 100%;
  margin-top: 2mm;
}

.cost-table {
  width: 100%;
  border-collapse: collapse;
  font-size: 7.5pt;
  border-radius: 1.2mm;
  overflow: hidden;
  border: 0.2mm solid rgba(0, 0, 0, 0.08);
  box-shadow: 0 0.5mm 1mm rgba(0, 0, 0, 0.02);
}

.cost-table th, 
.cost-table td {
  padding: 1.5mm 2mm;
  text-align: center;
  box-sizing: border-box;
}

.cost-table th {
  background-color: var(--primary-dark);
  color: #ffffff;
  font-weight: 700;
  font-size: 6.5pt;
  text-transform: uppercase;
}

.cost-table td {
  background-color: rgba(255, 255, 255, 0.7);
  font-weight: 500;
  color: #334155;
  border-top: 0.2mm solid rgba(0, 0, 0, 0.05);
}

.cost-table .row-label {
  font-weight: 700;
  text-align: left;
  background-color: rgba(255, 255, 255, 0.9);
  color: var(--primary-dark);
  font-size: 7pt;
}

.cost-table .cost-val {
  font-weight: 700;
  color: var(--primary-color);
  font-size: 8pt;
}

/* Right Column (60%) */
.lado-direito {
  width: 60%;
  box-sizing: border-box;
  padding-left: 4mm;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
}

.slots-header {
  font-size: 7pt;
  font-weight: 700;
  text-transform: uppercase;
  color: #64748b;
  letter-spacing: 0.8px;
  border-bottom: 0.25mm solid rgba(0,0,0,0.05);
  padding-bottom: 1.2mm;
  margin-bottom: 2mm;
}

.slots-grid {
  display: flex;
  flex-wrap: wrap;
  gap: 3mm;
  align-items: center;
  flex-grow: 1;
}

/* Slots (Dotted Rects with border: 2px dashed #ccc) */
.slot-box {
  box-sizing: border-box;
  border: 2px dashed #ccc; /* Gross dotted border exactly as requested */
  border-radius: 2.5mm;
  background-color: rgba(255, 255, 255, 0.4);
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  text-align: center;
  padding: 1.5mm;
  width: 22mm;  /* Exact square shape */
  height: 22mm; /* Exact square shape */
  transition: all 0.2s ease;
}

/* Dice Slot Specifics */
.slot-box-dado .slot-icon {
  font-size: 15pt;
  margin-bottom: 0.5mm;
}

.slot-box-dado .slot-label {
  font-size: 7pt;
  font-weight: 700;
  color: #475569;
  text-transform: uppercase;
}

/* Card Slot Specifics */
.slot-box-carta .slot-icon {
  font-size: 15pt;
  margin-bottom: 0.5mm;
}

.slot-box-carta .slot-label {
  font-size: 7pt;
  font-weight: 700;
  color: #475569;
  text-transform: uppercase;
  margin-bottom: 0.2mm;
}

.slot-box-carta .slot-sub {
  font-size: 5.5pt;
  color: var(--primary-dark);
  font-weight: 700;
  max-width: 100%;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
  text-transform: uppercase;
}

/* ============================================================= */
/* VERSO LAYOUT (Minimalist and Mysterious)                      */
/* ============================================================= */
.back-inner {
  padding: 3.5mm 5mm !important;
}

.back-mysterious-frame {
  width: 100%;
  height: 100%;
  box-sizing: border-box;
  border: 0.5mm solid rgba(0, 0, 0, 0.12);
  border-radius: 3mm;
  background: radial-gradient(circle at center, rgba(255, 255, 255, 0.15) 0%, rgba(0, 0, 0, 0.04) 100%);
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  padding: 4mm;
  gap: 3mm;
}

/* Back Top: Centered Profession Title */
.back-header {
  width: 100%;
  box-sizing: border-box;
  display: flex;
  flex-direction: column;
  align-items: center;
  text-align: center;
}

.back-header .profissao-title {
  font-size: 18pt;
  margin: 0;
  color: var(--primary-dark);
  letter-spacing: 0.5px;
}

.back-header .brand-tag {
  font-size: 7pt;
  margin-bottom: 1mm;
}

/* Back Bottom: Centered Illustration Box (Exact 21:9 Aspect Ratio) */
.back-illustration-box {
  width: 126mm;
  height: 54mm;
  box-sizing: border-box;
  border: 0.5mm solid var(--primary-color);
  border-radius: 2mm;
  overflow: hidden;
  position: relative;
  background-color: #f8fafc;
  box-shadow: 0 1mm 2mm rgba(0, 0, 0, 0.05);
}

.back-illustration-image {
  width: 100%;
  height: 100%;
  object-fit: cover;
  display: block;
}

/* Captions / Descrição da imagem visível em hover na tela */
.illustration-caption {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(15, 17, 21, 0.92);
  color: #e2e8f0;
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 4mm;
  box-sizing: border-box;
  opacity: 0;
  transition: opacity 0.25s ease;
  pointer-events: none;
  text-align: center;
}

.back-illustration-box:hover .illustration-caption {
  opacity: 1;
}

.illustration-caption p {
  font-size: 8pt;
  line-height: 1.4;
  font-style: italic;
  margin: 0;
}

/* ------------------------------------------------------------- */
/* PRINT STYLESHEET                                              */
/* ------------------------------------------------------------- */
@media print {
  /* Enforce canvas size & clear backgrounds */
  html, body {
    background: #ffffff !important;
    color: #000000 !important;
    width: 210mm !important;
    height: 297mm !important;
    margin: 0 !important;
    padding: 0 !important;
  }
  
  .player-boards-container {
    background: transparent !important;
    padding: 0 !important;
    margin: 0 !important;
    width: 210mm !important;
  }
  
  .no-print {
    display: none !important;
    height: 0 !important;
  }
  
  .printable-sheets {
    display: block !important;
    margin: 0 !important;
    padding: 0 !important;
    width: 210mm !important;
  }
  
  .page-a4 {
    box-shadow: none !important;
    margin: 0 !important;
    padding: 0 !important;
    border: none !important;
    width: 210mm !important;
    height: 297mm !important;
    display: flex !important;
    flex-direction: column !important;
    
    /* Strict page break rules */
    page-break-after: always !important;
    break-after: page !important;
    page-break-inside: avoid !important;
    break-inside: avoid !important;
  }
  
  .page-a4:last-child {
    page-break-after: avoid !important;
    break-after: avoid !important;
  }

  .ficha-row {
    width: 210mm !important;
    height: 99mm !important;
    display: flex !important;
    -webkit-print-color-adjust: exact !important;
    print-color-adjust: exact !important;
  }
  
  .ficha-inner {
    background: var(--bg-gradient) !important;
    border-color: var(--primary-color) !important;
  }
  
  .cost-table td {
    background-color: rgba(255, 255, 255, 0.7) !important;
  }
  
  .cost-table .row-label {
    background-color: rgba(255, 255, 255, 0.9) !important;
  }

  .slot-box {
    background-color: rgba(255, 255, 255, 0.45) !important;
  }
  
  .placeholder-design {
    background-color: rgba(255, 255, 255, 0.6) !important;
  }
}

@page {
  size: A4 portrait;
  margin: 0;
}
</style>
