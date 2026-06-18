<template>
  <div class="card-generator-container">
    <!-- Print controls visible only on screen -->
    <div class="print-controls no-print">
      <div class="header-content">
        <button @click="$emit('go-home')" class="btn-back">
          <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
            <line x1="19" y1="12" x2="5" y2="12"></line>
            <polyline points="12 19 5 12 12 5"></polyline>
          </svg>
          Voltar ao Início
        </button>
        <h1>Coleções Biológicas Fiocruz</h1>
        <p class="subtitle">Gerador de Cartas para o Jogo de Tabuleiro (SBPC 2026)</p>
      </div>
      <div class="info-and-actions">
        <div class="info-badge">
          <span class="count">{{ cartas.length }}</span> frentes / <span class="count">25</span> versos prontos para impressão
        </div>
        <button @click="exportJSON" class="btn-export" title="Exportar arquivo cartas_fiocruz.json atualizado">
          <svg xmlns="http://www.w3.org/2000/svg" width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
            <path d="M21 15v4a2 2 0 0 1-2-2H5a2 2 0 0 1-2-2v-4"></path>
            <polyline points="7 10 12 15 17 10"></polyline>
            <line x1="12" y1="15" x2="12" y2="3"></line>
          </svg>
          Exportar JSON
        </button>
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

    <!-- Printable A4 Sheets (Frentes e Versos intercalados por folha) -->
    <div class="printable-sheets">
      <template v-for="folha in folhasA4" :key="'folha-' + folha.id">
        
        <!-- PÁGINA A4: FRENTES (9 cartas por página) -->
        <div class="page-a4 page-fronts">
          <!-- Seção de Título apenas na tela -->
          <div class="section-title page-title-screen no-print">
            <h2>Folha {{ folha.id + 1 }} - Frente das Cartas</h2>
            <p>Clique em qualquer carta para abrir o painel de edição rápida</p>
          </div>

          <div class="cards-grid">
            <template v-for="(carta, index) in folha.frentes" :key="'front-' + folha.id + '-' + index">
              <!-- Placeholder invisível para alinhar o grid -->
              <div v-if="carta.isPlaceholder" class="card card-placeholder"></div>
              
              <!-- Carta Real (Frente) -->
              <div 
                v-else
                class="card card-front"
                :class="getCollectionClass(carta.colecao)"
                @click="openEditModal(carta.globalIndex)"
              >
                <div class="card-inner">
                  <header class="card-header">
                    <div class="collection-badge-wrapper">
                      <span class="collection-badge">{{ carta.colecao }}</span>
                    </div>
                    <h2 class="card-title">{{ carta.titulo }}</h2>
                    <h3 class="card-subtitle">{{ carta.subtitulo }}</h3>
                  </header>

                  <div class="edit-badge no-print">
                    <svg xmlns="http://www.w3.org/2000/svg" width="8" height="8" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5" stroke-linecap="round" stroke-linejoin="round">
                      <path d="M11 4H4a2 2 0 0 0-2 2v14a2 2 0 0 0 2 2h14a2 2 0 0 0 2-2v-7"></path>
                      <path d="M18.5 2.5a2.121 2.121 0 1 1 3 3L12 15l-4 1 1-4Z"></path>
                    </svg>
                  </div>

                  <div class="card-illustration">
                    <div class="illustration-border">
                      <img :src="'/imagens/' + carta.imagem" :alt="carta.descricao_ilustracao" class="illustration-image" />
                      <div class="illustration-overlay no-print">
                        <p class="overlay-desc">{{ carta.descricao_ilustracao }}</p>
                      </div>
                    </div>
                  </div>

                  <div class="card-action-box">
                    <span class="action-label">Ação</span>
                    <p class="action-desc">{{ carta.acao }}</p>
                    <div v-if="carta.simbolo" class="simbolo-container">
                      <div 
                        class="simbolo-icon"
                        :style="{ 
                          maskImage: `url('/imagens/${carta.simbolo}')`, 
                          webkitMaskImage: `url('/imagens/${carta.simbolo}')` 
                        }"
                      ></div>
                    </div>
                  </div>

                  <footer class="card-footer">
                    <p class="flavor-text">{{ carta.flavor_text }}</p>
                  </footer>
                </div>
              </div>
            </template>
          </div>
        </div>

        <!-- PÁGINA A4: VERSOS (9 cartas por página, espelhadas) -->
        <div class="page-a4 page-backs">
          <!-- Seção de Título apenas na tela -->
          <div class="section-title page-title-screen no-print">
            <h2>Folha {{ folha.id + 1 }} - Verso das Cartas</h2>
            <p>Espelhado horizontalmente para alinhamento duplex no verso</p>
          </div>

          <div class="cards-grid backs-grid">
            <template v-for="(carta, index) in folha.versos" :key="'back-' + folha.id + '-' + index">
              <!-- Placeholder invisível para alinhar o grid -->
              <div v-if="carta.isPlaceholder" class="card card-placeholder"></div>
              
              <!-- Carta Real (Verso) -->
              <div 
                v-else
                class="card card-back"
                :class="getCollectionClass(carta.colecao)"
              >
                <div class="card-inner back-inner">
                  <!-- Imagem de fundo temática para o verso da coleção -->
                  <img 
                    :src="'/imagens/' + getVersoImagem(carta.colecao)" 
                    class="back-bg-image" 
                    alt="Fundo do Verso"
                  />
                  <div class="back-pattern">
                    <!-- Emblem / Icon based on Collection -->
                    <div class="back-emblem">
                      <svg v-if="carta.colecao === 'Histopatológica'" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="icon-emblem">
                        <circle cx="12" cy="12" r="10"></circle>
                        <circle cx="12" cy="12" r="3"></circle>
                        <circle cx="6" cy="9" r="1"></circle>
                        <circle cx="17" cy="8" r="1.5"></circle>
                        <circle cx="9" cy="16" r="2"></circle>
                      </svg>
                      <svg v-else-if="carta.colecao === 'Microbiológica'" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="icon-emblem">
                        <circle cx="12" cy="12" r="10"></circle>
                        <path d="M12 6a6 6 0 0 1 6 6c0 2.2-1.8 4-4 4s-4-1.8-4-4S12 6 12 6z"></path>
                        <path d="M8 8l1.5 1.5M16 8l-1.5 1.5M8 16l1.5-1.5M16 16l-1.5-1.5"></path>
                      </svg>
                      <svg v-else-if="carta.colecao === 'Zoológica'" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="icon-emblem">
                        <path d="M12 2v20M12 10a4 4 0 1 0 0-8 4 4 0 0 0 0 8zm0 12a4 4 0 1 0 0-8 4 4 0 0 0 0 8z"></path>
                        <path d="M2 12h20M6 8l12 8M6 16l12-8"></path>
                      </svg>
                      <svg v-else-if="carta.colecao === 'Botânica'" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="icon-emblem">
                        <path d="M2 22C2 22 8 18 12 12C16 6 22 2 22 2C22 2 18 8 12 12C6 16 2 22 2 22Z"></path>
                        <path d="M12 12l4 1M8 16l3 1M16 8l1 4M19 5l-3 1"></path>
                      </svg>
                      <svg v-else-if="carta.colecao === 'Arqueopaleontológica'" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="icon-emblem">
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
                      <span class="back-sub">{{ carta.colecao }}</span>
                    </div>
                  </div>
                  
                  <div class="back-footer">
                    <span>SBPC 2026 • Jogo de Tabuleiro</span>
                  </div>
                </div>
              </div>
            </template>
          </div>
        </div>

      </template>
    </div>

    <!-- Edit Card Modal Panel (Screen view only) -->
    <div v-if="isModalOpen" class="modal-overlay no-print" @click.self="closeModal">
      <div class="modal-content" :class="getCollectionClass(editForm.colecao)">
        <header class="modal-header">
          <h2>Editar Conteúdo da Carta</h2>
          <span class="modal-collection">{{ editForm.colecao }}</span>
        </header>
        
        <div class="modal-body">
          <div class="form-group">
            <label for="edit-title">Título</label>
            <input id="edit-title" v-model="editForm.titulo" type="text" placeholder="Nome do item/recurso" />
          </div>
          
          <div class="form-group">
            <label for="edit-subtitle">Subtítulo</label>
            <input id="edit-subtitle" v-model="editForm.subtitulo" type="text" placeholder="Tipo do recurso" />
          </div>
          
          <div class="form-group">
            <label for="edit-action">Ação da Carta (Dado)</label>
            <input id="edit-action" v-model="editForm.acao" type="text" placeholder="Poder/Efeito no dado" />
          </div>

          <div class="form-group">
            <label for="edit-image">Nome da Imagem (.png)</label>
            <input id="edit-image" v-model="editForm.imagem" type="text" placeholder="ex: histopatologia_01.png" />
          </div>
          
          <div class="form-group">
            <label for="edit-simbolo">Símbolo Especial (.svg)</label>
            <input id="edit-simbolo" v-model="editForm.simbolo" type="text" placeholder="ex: celula.svg (deixe em branco se nenhum)" />
          </div>

          <div class="form-group">
            <label for="edit-desc">Descrição da Ilustração (Hover)</label>
            <textarea id="edit-desc" v-model="editForm.descricao_ilustracao" rows="3" placeholder="Descrição do desenho na carta"></textarea>
          </div>
          
          <div class="form-group">
            <label for="edit-flavor">Texto de Ambientação (Flavor Text)</label>
            <textarea id="edit-flavor" v-model="editForm.flavor_text" rows="3" placeholder="Frase em itálico de ambientação"></textarea>
          </div>
        </div>
        
        <footer class="modal-footer">
          <button @click="closeModal" class="btn-cancel">Cancelar</button>
          <button @click="saveCard" class="btn-save">Salvar Alterações</button>
        </footer>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue';
import rawCartas from './cartas_fiocruz.json';

defineEmits(['go-home']);

const colecoes = [
  'Histopatológica',
  'Microbiológica',
  'Zoológica',
  'Botânica',
  'Arqueopaleontológica'
];

// Reactive array of cards
const cartas = ref([...rawCartas]);

// Modal State
const isModalOpen = ref(false);
const activeIndex = ref(null);
const editForm = ref({
  colecao: '',
  titulo: '',
  subtitulo: '',
  descricao_ilustracao: '',
  acao: '',
  imagem: '',
  simbolo: '',
  flavor_text: ''
});

// Open and load card details into edit form
const openEditModal = (index) => {
  activeIndex.value = index;
  editForm.value = { ...cartas.value[index] };
  isModalOpen.value = true;
};

// Close edit panel
const closeModal = () => {
  isModalOpen.value = false;
  activeIndex.value = null;
};

// Save edited card properties back to reactive array
const saveCard = () => {
  if (activeIndex.value !== null) {
    cartas.value[activeIndex.value] = { ...editForm.value };
  }
  closeModal();
};

// Export modified cards as a new cartas_fiocruz.json file
const exportJSON = () => {
  const dataStr = "data:text/json;charset=utf-8," + encodeURIComponent(JSON.stringify(cartas.value, null, 2));
  const downloadAnchor = document.createElement('a');
  downloadAnchor.setAttribute("href", dataStr);
  downloadAnchor.setAttribute("download", "cartas_fiocruz.json");
  document.body.appendChild(downloadAnchor);
  downloadAnchor.click();
  downloadAnchor.remove();
};

// Agrupa as cartas em páginas A4, intercalando uma folha de frentes com uma folha de versos
const folhasA4 = computed(() => {
  const list = [...cartas.value];
  const pages = [];
  const cardsPerPage = 9;
  const cols = 3;
  
  for (let i = 0; i < list.length; i += cardsPerPage) {
    const chunk = list.slice(i, i + cardsPerPage);
    
    // Completa a página com placeholders se necessário
    while (chunk.length < cardsPerPage) {
      chunk.push({ isPlaceholder: true, id: 'placeholder-' + i + '-' + chunk.length });
    }
    
    // 1. Criar a página de frentes (ordem normal 0 a 8)
    const frentes = chunk.map((c, idx) => ({
      ...c,
      globalIndex: i + idx // Guarda o índice global real para edição
    }));
    
    // 2. Criar a página de versos (espelhada horizontalmente, ou seja, reverte a ordem de cada linha de 3 colunas)
    const versos = [];
    for (let r = 0; r < cardsPerPage; r += cols) {
      const row = chunk.slice(r, r + cols);
      row.reverse(); // Inverte a ordem das colunas para alinhamento duplex
      versos.push(...row);
    }
    
    pages.push({
      id: i / cardsPerPage,
      frentes,
      versos
    });
  }
  return pages;
});

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

const getVersoImagem = (colecao) => {
  const mapping = {
    'Histopatológica': 'verso_histopatologica.png',
    'Microbiológica': 'verso_microbiologica.png',
    'Zoológica': 'verso_zoologica.png',
    'Botânica': 'verso_botanica.png',
    'Arqueopaleontológica': 'verso_arqueopaleontologica.png'
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
  gap: 12px;
}

.info-badge {
  background-color: rgba(255, 255, 255, 0.04);
  border: 1px solid rgba(255, 255, 255, 0.1);
  padding: 8px 16px;
  border-radius: 9999px;
  font-size: 14px;
  color: #cbd5e1;
  margin-right: 8px;
}

.info-badge .count {
  font-weight: 700;
  color: #38bdf8;
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
  margin-bottom: 12px;
}

.btn-back:hover {
  background: rgba(255, 255, 255, 0.05);
  color: #f8fafc;
}

.btn-export {
  background: rgba(255, 255, 255, 0.04);
  border: 1px solid rgba(255, 255, 255, 0.1);
  color: #f8fafc;
  border-radius: 8px;
  padding: 10px 18px;
  font-weight: 600;
  font-size: 14px;
  cursor: pointer;
  display: flex;
  align-items: center;
  gap: 8px;
  transition: all 0.2s cubic-bezier(0.4, 0, 0.2, 1);
}

.btn-export:hover {
  background: rgba(255, 255, 255, 0.08);
  border-color: rgba(255, 255, 255, 0.2);
  transform: translateY(-2px);
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
/* Printable container for screen view */
.printable-sheets {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 40px;
  width: 100%;
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
  justify-content: center;
  align-items: center;
  padding: 10mm;
  box-shadow: 0 15px 40px rgba(0, 0, 0, 0.45);
  border-radius: 4px;
  
  /* Print breaks */
  page-break-after: always;
  break-after: page;
  -webkit-print-color-adjust: exact;
  print-color-adjust: exact;
}

.page-title-screen {
  position: absolute;
  top: 3mm;
  left: 10mm;
  border-left: 3px solid #38bdf8;
  padding-left: 8px;
  margin: 0;
}

.page-title-screen h2 {
  font-size: 11px;
  text-transform: uppercase;
  color: #64748b;
  margin: 0;
  letter-spacing: 0.5px;
}

.page-title-screen p {
  font-size: 8px;
  color: #94a3b8;
  margin: 0;
}

.page-backs .page-title-screen {
  border-left-color: #a855f7;
}

.cards-grid {
  display: grid;
  grid-template-columns: repeat(3, 59mm);
  grid-template-rows: repeat(3, 85mm);
  gap: 4mm;
  justify-content: center;
  align-content: center;
  width: 100%;
  height: 100%;
  box-sizing: border-box;
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

/* Card Front Click Styling */
.card-front {
  cursor: pointer;
}

.card-front::after {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(255, 255, 255, 0);
  transition: background 0.2s ease;
  pointer-events: none;
}

.card-front:hover::after {
  background: rgba(255, 255, 255, 0.05);
}

.card:hover {
  transform: translateY(-6px);
  box-shadow: 0 12px 30px var(--border-glow);
}

/* Edit Icon Badge overlay */
.edit-badge {
  position: absolute;
  top: 1.5mm;
  right: 1.5mm;
  background: rgba(15, 17, 21, 0.85);
  border: 0.3mm solid rgba(255, 255, 255, 0.15);
  color: #ffffff;
  border-radius: 50%;
  width: 6mm;
  height: 6mm;
  display: flex;
  align-items: center;
  justify-content: center;
  opacity: 0;
  transition: opacity 0.2s cubic-bezier(0.4, 0, 0.2, 1);
  pointer-events: none;
  z-index: 5;
  box-shadow: 0 1px 3px rgba(0, 0, 0, 0.3);
}

.card-front:hover .edit-badge {
  opacity: 1;
}

.edit-badge svg {
  width: 3.2mm;
  height: 3.2mm;
  stroke: #38bdf8;
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

.simbolo-container {
  display: flex;
  justify-content: center;
  align-items: center;
  margin-top: 1.2mm;
}

.simbolo-icon {
  background-color: var(--primary-color);
  width: 5.5mm;
  height: 5.5mm;
  display: inline-block;
  mask-size: contain;
  mask-position: center;
  mask-repeat: no-repeat;
  -webkit-mask-size: contain;
  -webkit-mask-position: center;
  -webkit-mask-repeat: no-repeat;
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
  position: relative;
  overflow: hidden;
  border: 0.3mm solid rgba(255, 255, 255, 0.15);
  background: radial-gradient(circle at center, rgba(0, 0, 0, 0.35) 0%, rgba(0, 0, 0, 0.82) 100%) !important;
  color: #ffffff;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  align-items: center;
  padding: 4mm 2mm 3mm 2mm !important;
  box-sizing: border-box;
}

.back-bg-image {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  object-fit: cover;
  opacity: 0.45;
  filter: saturate(0.85) brightness(0.65) contrast(1.1);
  z-index: 1;
  pointer-events: none;
}

.back-pattern {
  position: relative;
  z-index: 2;
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
  position: relative;
  z-index: 2;
  border-top: 0.25mm solid rgba(255, 255, 255, 0.15);
  width: 100%;
  padding-top: 2.2mm;
  text-align: center;
  background: rgba(0, 0, 0, 0.3);
  margin-top: 2mm;
  padding-bottom: 0.5mm;
  border-radius: 0 0 1mm 1mm;
}

.back-footer span {
  font-size: 4.8pt;
  letter-spacing: 0.4px;
  color: var(--primary-light);
  opacity: 0.65;
  text-transform: uppercase;
  font-weight: 500;
}

/* Placeholder card for print mapping alignment */
.card-placeholder {
  width: 59mm;
  height: 85mm;
  box-sizing: border-box;
  border: 3.5mm solid transparent;
  padding: 1.2mm;
  visibility: hidden;
  background: transparent !important;
  box-shadow: none !important;
  page-break-inside: avoid;
  break-inside: avoid;
}

/* ------------------------------------------------------------- */
/* EDIT MODAL DIALOG STYLE (Screen only)                         */
/* ------------------------------------------------------------- */
.modal-overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100vw;
  height: 100vh;
  background: rgba(10, 12, 16, 0.82);
  backdrop-filter: blur(8px);
  z-index: 999;
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 20px;
  box-sizing: border-box;
}

.modal-content {
  background: #181d28;
  border: 1px solid rgba(255, 255, 255, 0.08);
  border-top: 5px solid var(--primary-color, #38bdf8);
  border-radius: 20px;
  width: 100%;
  max-width: 520px;
  box-shadow: 0 24px 60px rgba(0, 0, 0, 0.75), 0 0 40px var(--border-glow);
  display: flex;
  flex-direction: column;
  overflow: hidden;
  animation: modalEnter 0.22s cubic-bezier(0.34, 1.56, 0.64, 1);
}

@keyframes modalEnter {
  from {
    opacity: 0;
    transform: translateY(30px) scale(0.94);
  }
  to {
    opacity: 1;
    transform: translateY(0) scale(1);
  }
}

.modal-header {
  padding: 20px 24px;
  border-bottom: 1px solid rgba(255, 255, 255, 0.06);
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.modal-header h2 {
  font-family: 'Cinzel', serif;
  font-size: 19px;
  margin: 0;
  color: #f8fafc;
  letter-spacing: 0.5px;
}

.modal-collection {
  font-size: 10.5px;
  font-weight: 700;
  text-transform: uppercase;
  color: #ffffff;
  background-color: var(--primary-color);
  padding: 4px 12px;
  border-radius: 6px;
  letter-spacing: 0.5px;
  box-shadow: 0 2px 8px var(--border-glow);
}

.modal-body {
  padding: 24px;
  overflow-y: auto;
  max-height: calc(100vh - 200px);
  display: flex;
  flex-direction: column;
  gap: 16px;
  background: #141822;
}

.form-group {
  display: flex;
  flex-direction: column;
  gap: 6px;
}

.form-group label {
  font-size: 12px;
  font-weight: 600;
  color: #94a3b8;
  letter-spacing: 0.3px;
}

.form-group input,
.form-group textarea {
  background: #0f1118;
  border: 1px solid rgba(255, 255, 255, 0.08);
  border-radius: 10px;
  padding: 12px;
  color: #f1f5f9;
  font-family: 'Outfit', sans-serif;
  font-size: 14px;
  transition: all 0.2s cubic-bezier(0.4, 0, 0.2, 1);
}

.form-group input:focus,
.form-group textarea:focus {
  outline: none;
  border-color: var(--primary-color);
  box-shadow: 0 0 12px var(--border-glow);
  background: #11141d;
}

.modal-footer {
  padding: 18px 24px;
  background: #181d28;
  border-top: 1px solid rgba(255, 255, 255, 0.06);
  display: flex;
  justify-content: flex-end;
  gap: 12px;
}

.btn-cancel {
  background: transparent;
  color: #94a3b8;
  border: 1px solid rgba(255, 255, 255, 0.08);
  padding: 10px 20px;
  border-radius: 8px;
  font-weight: 600;
  font-size: 14px;
  cursor: pointer;
  transition: all 0.2s cubic-bezier(0.4, 0, 0.2, 1);
}

.btn-cancel:hover {
  background: rgba(255, 255, 255, 0.04);
  color: #f8fafc;
  border-color: rgba(255, 255, 255, 0.15);
}

.btn-save {
  background: var(--primary-color);
  color: #ffffff;
  border: none;
  padding: 10px 22px;
  border-radius: 8px;
  font-weight: 600;
  font-size: 14px;
  cursor: pointer;
  transition: all 0.2s cubic-bezier(0.4, 0, 0.2, 1);
  box-shadow: 0 4px 12px var(--border-glow);
}

.btn-save:hover {
  background: var(--accent-color);
  transform: translateY(-2px);
  box-shadow: 0 6px 18px var(--border-glow);
}

.btn-save:active {
  transform: translateY(0);
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
    width: 210mm !important;
    height: 297mm !important;
    margin: 0 !important;
    padding: 0 !important;
  }
  
  .card-generator-container {
    background: transparent !important;
    padding: 0 !important;
    margin: 0 !important;
    width: 210mm !important;
  }
  
  /* Hide web controls completely */
  .no-print {
    display: none !important;
    height: 0 !important;
    margin: 0 !important;
    padding: 0 !important;
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
    border: none !important;
    width: 210mm !important;
    height: 297mm !important;
    padding: 10mm !important;
    display: flex !important;
    flex-direction: column !important;
    justify-content: center !important;
    align-items: center !important;
    
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

  /* Layout the grid to fit A4 perfectly */
  .cards-grid {
    display: grid !important;
    grid-template-columns: repeat(3, 59mm) !important;
    grid-template-rows: repeat(3, 85mm) !important;
    gap: 4mm !important;
    padding: 0 !important;
    margin: 0 !important;
    justify-content: center !important;
    align-content: center !important;
    width: 100% !important;
    height: 100% !important;
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
    background: radial-gradient(circle at center, rgba(0, 0, 0, 0.35) 0%, rgba(0, 0, 0, 0.82) 100%) !important;
    color: #ffffff !important;
  }
}

/* Define A4 page dimensions and reset margins */
@page {
  size: A4 portrait;
  margin: 0;
}
</style>
