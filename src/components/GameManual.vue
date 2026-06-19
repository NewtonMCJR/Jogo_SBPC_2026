<template>
  <div class="manual-container">
    <!-- Screen Header (Controls & Title) -->
    <header class="manual-header no-print">
      <div class="header-inner">
        <button @click="$emit('go-home')" class="btn-back">
          <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
            <line x1="19" y1="12" x2="5" y2="12"></line>
            <polyline points="12 19 5 12 12 5"></polyline>
          </svg>
          Voltar ao Início
        </button>
        <div class="title-area">
          <span class="brand-tag">SBPC 2026 • Museu da Patologia</span>
          <h1>Manual de Instruções</h1>
          <p class="subtitle">Mistérios da Ciência: O Jogo das Coleções Biológicas da Fiocruz</p>
        </div>
      </div>
      <div class="header-actions">
        <button @click="printManual" class="btn-print">
          <svg xmlns="http://www.w3.org/2000/svg" width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
            <polyline points="6 9 6 2 18 2 18 9"></polyline>
            <path d="M6 18H4a2 2 0 0 1-2-2v-5a2 2 0 0 1 2-2h16a2 2 0 0 1 2 2v5a2 2 0 0 1-2 2h-2"></path>
            <rect x="6" y="14" width="12" height="8"></rect>
          </svg>
          Imprimir Manual (A4)
        </button>
      </div>
    </header>

    <!-- Main Content Layout (Sidebar + Main Article) -->
    <div class="manual-layout">
      <!-- Quick Navigation Sidebar (Hidden on print) -->
      <nav class="manual-sidebar no-print">
        <div class="sidebar-header">
          <span>Tópicos do Guia</span>
        </div>
        <ul class="nav-links">
          <li><a href="#intro" class="nav-link" :class="{ active: activeSection === 'intro' }">📖 1. Bem-vindos, Cientistas!</a></li>
          <li><a href="#objective" class="nav-link" :class="{ active: activeSection === 'objective' }">🎯 2. Objetivo do Jogo</a></li>
          <li><a href="#components" class="nav-link" :class="{ active: activeSection === 'components' }">📦 3. Componentes</a></li>
          <li><a href="#setup" class="nav-link" :class="{ active: activeSection === 'setup' }">⚙️ 4. Preparação (Setup)</a></li>
          <li><a href="#professions" class="nav-link" :class="{ active: activeSection === 'professions' }">🧬 5. Profissões da Fiocruz</a></li>
          <li><a href="#gameplay" class="nav-link" :class="{ active: activeSection === 'gameplay' }">🎲 6. Como Jogar (Turno)</a></li>
          <li><a href="#endgame" class="nav-link" :class="{ active: activeSection === 'endgame' }">🏆 7. Fim de Jogo</a></li>
        </ul>
      </nav>

      <!-- Main Document -->
      <main class="manual-document">
        
        <!-- SECTION 1: INTRO -->
        <section id="intro" class="doc-section">
          <div class="section-title-wrapper">
            <span class="section-num">01</span>
            <h2>Mistérios da Ciência: O Jogo</h2>
          </div>
          <p class="lead-text"> Uma aventura cooperativa pelas Coleções Biológicas da Fiocruz </p>
          
          <div class="game-meta-cards">
            <div class="meta-card">
              <span class="meta-icon">👥</span>
              <span class="meta-label">Jogadores</span>
              <span class="meta-val">2 a 5 Jogadores</span>
            </div>
            <div class="meta-card">
              <span class="meta-icon">🎓</span>
              <span class="meta-label">Público-alvo</span>
              <span class="meta-val">Ensino Fundamental II e Médio</span>
            </div>
            <div class="meta-card">
              <span class="meta-icon">🤝</span>
              <span class="meta-label">Modo</span>
              <span class="meta-val">Cooperativo</span>
            </div>
          </div>

          <div class="dialog-box">
            <p>
              <strong>Bem-vindos, cientistas!</strong> Em <em>Mistérios da Ciência</em>, vocês são pesquisadores que colaboram para solucionar desafios e enigmas que afetam a saúde pública e o meio ambiente. Usando a lógica, o planejamento estratégico e os recursos valiosos das Coleções Biológicas da Fiocruz, vocês devem trabalhar em equipe para desvendar todos os mistérios. Este é um jogo cooperativo criado pelo Museu da Patologia da Fiocruz para explorar o fascinante mundo da pesquisa científica.
            </p>
          </div>
        </section>

        <!-- SECTION 2: OBJECTIVE -->
        <section id="objective" class="doc-section">
          <div class="section-title-wrapper">
            <span class="section-num">02</span>
            <h2>Objetivo do Jogo</h2>
          </div>
          <p>
            O objetivo da equipe é trabalhar em conjunto para resolver todos os "casos" presentes no tabuleiro antes que os recursos de pesquisa (cartas de baralho) se esgotem totalmente. Para vencer, vocês devem cooperar para virar todas as <strong>12 peças hexagonais de território</strong> do mapa para o seu lado de <strong>"solução encontrada"</strong>.
          </p>
        </section>

        <!-- SECTION 3: COMPONENTS -->
        <section id="components" class="doc-section">
          <div class="section-title-wrapper">
            <span class="section-num">03</span>
            <h2>Componentes do Jogo</h2>
          </div>
          
          <div class="components-grid">
            <div class="comp-item-card">
              <h3>🛑 12 Peças de Território Hexagonais</h3>
              <p>Peças de dupla face representando regiões do mapa. O lado da frente mostra um <strong>"problema"</strong> de saúde pública, a quantidade de dados exigida e a regra de combinação de símbolos. O verso revela a <strong>"solução"</strong> científica e a recompensa em Pontos de Financiamento (+2, +4 ou +6).</p>
            </div>
            
            <div class="comp-item-card">
              <h3>🎲 6 Dados Científicos (Customizados)</h3>
              <p>Dados especiais de 6 lados representando áreas e materiais da pesquisa biológica:</p>
              <ul class="bullet-list">
                <li><strong>Célula</strong> (Histopatologia)</li>
                <li><strong>Bactéria</strong> (Microbiologia)</li>
                <li><strong>Mosquito</strong> (Zoologia)</li>
                <li><strong>Folha</strong> (Botânica)</li>
                <li><strong>Coprólito</strong> (Arqueopaleontologia)</li>
                <li><strong>Seringa</strong> (Coringa / Vacina)</li>
              </ul>
            </div>

            <div class="comp-item-card">
              <h3>🎲 1 Dado Comunitário</h3>
              <p>Um dado de cor de destaque usado exclusivamente para rolagens extraordinárias que podem ser compradas pela equipe ao longo da partida utilizando pontos de financiamento.</p>
            </div>

            <div class="comp-item-card">
              <h3>📐 5 Pranchetas de Profissão</h3>
              <p>Fichas assimétricas de personagens que determinam a quantidade inicial de dados base e o limite de cartas na mão de cada jogador.</p>
            </div>

            <div class="comp-item-card">
              <h3>🃏 25 Cartas de Coleções Biológicas</h3>
              <p>Divididas em 5 baralhos de 5 cartas cada, contendo ações especiais e ilustrações exclusivas em 21:9 das coleções da Fiocruz: <em>Histopatológica</em>, <em>Microbiológica</em>, <em>Zoológica</em>, <em>Botânica</em> e <em>Arqueopaleontológica</em>.</p>
            </div>

            <div class="comp-item-card">
              <h3>🪙 Marcadores de Financiamento</h3>
              <p>Fichas ou tokens para contabilizar os pontos acumulados pela equipe e armazenados no Banco Comunitário.</p>
            </div>
          </div>
        </section>

        <!-- SECTION 4: SETUP -->
        <section id="setup" class="doc-section">
          <div class="section-title-wrapper">
            <span class="section-num">04</span>
            <h2>Preparação do Jogo (Setup)</h2>
          </div>
          
          <ol class="setup-list">
            <li>
              <div class="setup-step-header">O Território</div>
              <p>Embaralhem e disponham as 12 peças hexagonais no centro da mesa, formando o mapa da região, com o lado do <strong>"problema"</strong> (vermelho/alerta) virado para cima.</p>
            </li>
            <li>
              <div class="setup-step-header">Os Acervos</div>
              <p>Separem as cartas em 5 baralhos de acordo com a sua Coleção Biológica. Embaralhem cada monte separadamente e coloquem-nos virados para baixo, ao alcance de todos os jogadores.</p>
            </li>
            <li>
              <div class="setup-step-header">A Equipe</div>
              <p>Cada jogador escolhe uma Prancheta de Profissão (ou sorteia usando as fichas com o verso virado para cima). Em partidas com <strong>2 jogadores</strong>, cada participante deve escolher e controlar <strong>2 profissões simultaneamente</strong> para garantir o equilíbrio de recursos na mesa.</p>
            </li>
            <li>
              <div class="setup-step-header">Distribuição de Recursos</div>
              <p>Cada jogador pega a quantidade de dados base indicada na sua prancheta. Em seguida, compra cartas do topo dos baralhos permitidos pela sua profissão até atingir o seu "Limite de Cartas" pessoal.</p>
            </li>
            <li>
              <div class="setup-step-header">O Banco de Financiamento</div>
              <p>Coloquem os Marcadores de Financiamento em uma pilha geral (Banco Geral). A equipe começa a partida com <strong>0 pontos</strong> no seu Banco Comunitário.</p>
            </li>
            <li>
              <div class="setup-step-header">O Início</div>
              <p>Escolham o jogador que irá começar a primeira rodada (sugestão: o cientista que visitou um museu ou laboratório mais recentemente).</p>
            </li>
          </ol>
        </section>

        <!-- SECTION 5: PROFESSIONS -->
        <section id="professions" class="doc-section">
          <div class="section-title-wrapper">
            <span class="section-num">05</span>
            <h2>As Profissões da Fiocruz</h2>
          </div>
          <p class="section-intro">
            Cada profissional possui competências assimétricas fundamentadas e acesso restrito a determinados acervos biológicos de pesquisa:
          </p>

          <div class="professions-list">
            <!-- Biólogo -->
            <div class="profession-card theme-zoologica">
              <div class="prof-header">
                <span class="prof-icon">🌳</span>
                <div>
                  <h3>Biólogo</h3>
                  <span class="prof-label">Foco: Alta Manipulação Tática</span>
                </div>
              </div>
              <div class="prof-details">
                <div class="prof-stats">
                  <span><strong>Dados Base:</strong> 1 🎲</span>
                  <span><strong>Limite de Cartas:</strong> 2 🃏</span>
                </div>
                <p class="prof-text"><strong>Acervos Permitidos:</strong> Botânica ou Zoológica. Pode retirar recursos exclusivamente destes dois acervos. Suas cartas permitem re-rolagens extras e alterações táticas versáteis.</p>
              </div>
            </div>

            <!-- Microbiologista -->
            <div class="profession-card theme-microbiologica">
              <div class="prof-header">
                <span class="prof-icon">🧫</span>
                <div>
                  <h3>Microbiologista</h3>
                  <span class="prof-label">Foco: Força Bruta de Dados</span>
                </div>
              </div>
              <div class="prof-details">
                <div class="prof-stats">
                  <span><strong>Dados Base:</strong> 2 🎲</span>
                  <span><strong>Limite de Cartas:</strong> 1 🃏</span>
                </div>
                <p class="prof-text"><strong>Acervos Permitidos:</strong> Microbiológica. Retira recursos apenas deste acervo. Especialista em gerar e alocar múltiplos dados em simultâneo no tabuleiro.</p>
              </div>
            </div>

            <!-- Patologista -->
            <div class="profession-card theme-histopatologica">
              <div class="prof-header">
                <span class="prof-icon">🔬</span>
                <div>
                  <h3>Patologista</h3>
                  <span class="prof-label">Foco: Fechamento Rápido</span>
                </div>
              </div>
              <div class="prof-details">
                <div class="prof-stats">
                  <span><strong>Dados Base:</strong> 2 🎲</span>
                  <span><strong>Limite de Cartas:</strong> 1 🃏</span>
                </div>
                <p class="prof-text"><strong>Acervos Permitidos:</strong> Histopatológica. Retira recursos apenas deste acervo. Excelente em acelerar o diagnóstico celular e solucionar mistérios complexos.</p>
              </div>
            </div>

            <!-- Paleontólogo -->
            <div class="profession-card theme-arqueopaleontologica">
              <div class="prof-header">
                <span class="prof-icon">🦴</span>
                <div>
                  <h3>Paleontólogo</h3>
                  <span class="prof-label">Foco: Flexibilidade e Resgate</span>
                </div>
              </div>
              <div class="prof-details">
                <div class="prof-stats">
                  <span><strong>Dados Base:</strong> 1 🎲</span>
                  <span><strong>Limite de Cartas:</strong> 2 🃏</span>
                </div>
                <p class="prof-text"><strong>Acervos Permitidos:</strong> Arqueopaleontológica. Retira recursos exclusivamente do acervo fóssil. Habilidoso em manipular o descarte e ativar efeitos históricos.</p>
              </div>
            </div>

            <!-- Epidemiologista -->
            <div class="profession-card theme-epidemiologista">
              <div class="prof-header">
                <span class="prof-icon">📊</span>
                <div>
                  <h3>Epidemiologista</h3>
                  <span class="prof-label">Foco: Suporte e Integração (Coringa)</span>
                </div>
              </div>
              <div class="prof-details">
                <div class="prof-stats">
                  <span><strong>Dados Base:</strong> 1 🎲</span>
                  <span><strong>Limite de Cartas:</strong> 1 🃏</span>
                </div>
                <p class="prof-text"><strong>Acervos Permitidos:</strong> Qualquer Coleção. O epidemiologista é o elo da equipe: pode retirar cartas de qualquer baralho para abastecer as necessidades imediatas do grupo.</p>
              </div>
            </div>
          </div>
        </section>

        <!-- SECTION 6: GAMEPLAY -->
        <section id="gameplay" class="doc-section">
          <div class="section-title-wrapper">
            <span class="section-num">06</span>
            <h2>Como Jogar</h2>
          </div>
          <p>
            O jogo acontece em turnos, com os jogadores atuando em sequência no sentido horário. No seu turno, o cientista ativo realiza os seguintes passos de pesquisa:
          </p>

          <div class="steps-container">
            <!-- Passo 1 -->
            <div class="game-step">
              <div class="step-badge">Passo 1</div>
              <h3>A Rolagem de Pesquisa</h3>
              <p>No início do seu turno, role todos os dados disponíveis na sua reserva pessoal (seus dados base + qualquer dado comunitário temporário que tenha sido comprado no início do seu turno). Os símbolos obtidos representam as evidências e ferramentas científicas deste turno.</p>
            </div>

            <!-- Passo 2 -->
            <div class="game-step">
              <div class="step-badge">Passo 2</div>
              <h3>O Uso dos Acervos (Cartas)</h3>
              <p>Antes de alocar os dados no tabuleiro, você ou o grupo podem decidir jogar cartas da sua mão para manipular os resultados obtidos. Leia o texto da carta em voz alta, aplique o efeito (como <em>Mudar um símbolo</em>, <em>Re-rolar dados</em> ou <em>Virar para a face oposta</em>) e descarte-a em seguida. Não há limite de quantas cartas podem ser usadas por turno.</p>
            </div>

            <!-- Passo 3 -->
            <div class="game-step">
              <div class="step-badge">Passo 3</div>
              <h3>Financiamento de Emergência (Dado Comunitário)</h3>
              <p>Se precisar de mais recursos para resolver um enigma complexo, o jogador ativo pode gastar os pontos acumulados no Banco Comunitário para rolar o Dado Comunitário Extra. Este dado é devolvido ao centro da mesa no fim do seu turno. O custo para solicitar este esforço extra aumenta a cada compra efetuada pela equipe:</p>
              
              <div class="cost-progression-box">
                <div class="cost-node">
                  <span class="req">1ª Compra</span>
                  <span class="val">2 pts</span>
                </div>
                <div class="cost-arrow">➔</div>
                <div class="cost-node">
                  <span class="req">2ª Compra</span>
                  <span class="val">4 pts</span>
                </div>
                <div class="cost-arrow">➔</div>
                <div class="cost-node">
                  <span class="req">3ª Compra</span>
                  <span class="val">8 pts</span>
                </div>
                <div class="cost-arrow">➔</div>
                <div class="cost-node">
                  <span class="req">4ª Compra</span>
                  <span class="val">16 pts</span>
                </div>
              </div>
              <p class="cost-note">A partir da 4ª compra, o custo continua dobrando progressivamente (32, 64, etc.).</p>
            </div>

            <!-- Passo 4 -->
            <div class="game-step">
              <div class="step-badge">Passo 4</div>
              <h3>Resolução de Territórios</h3>
              <p>O jogador ativo aloca os seus dados sobre as peças de território para tentar preencher os requisitos do enigma. A indicação na peça (ex: 2x, 3x, 4x) dita o número mínimo de dados necessários para fechar o caso. A combinação dos símbolos deve seguir as regras de validação do território:</p>
              
              <div class="rules-highlight">
                <div class="rule-box">
                  <h4>Regra "X" (Variabilidade)</h4>
                  <p>Todos os dados alocados nos espaços contendo "X" devem ter obrigatoriamente <strong>símbolos diferentes entre si</strong>. Representa a necessidade de abordagens científicas multidisciplinares.</p>
                </div>
                <div class="rule-box">
                  <h4>Regra "=" (Consistência)</h4>
                  <p>Todos os dados alocados nos espaços contendo "=" devem ter o <strong>mesmo símbolo idêntico</strong>. Representa a necessidade de precisão e replicação laboratorial.</p>
                </div>
              </div>

              <div class="coringa-box">
                <strong>A Seringa (Coringa):</strong> O símbolo da Seringa representa o avanço da vacina ou do tratamento definitivo. Ele substitui qualquer símbolo de coleção necessário para validar uma regra (tanto no "X" quanto no "=").
              </div>

              <div class="teamwork-box">
                💡 <strong>Alocação Conjunta (Trabalho em Equipe):</strong> Se um território exigir mais dados do que você possui no turno, você pode alocar os dados que já correspondem à regra e deixá-los posicionados sobre o hexágono. Nos turnos seguintes, os seus colegas de equipe podem adicionar os dados que faltam para completar o desafio!
              </div>
            </div>

            <!-- Passo 5 -->
            <div class="game-step">
              <div class="step-badge">Passo 5</div>
              <h3>Conclusão do Caso e Recompensa</h3>
              <p>Quando o último espaço de dado de uma peça de território for preenchido, o caso é solucionado! Realize os seguintes passos:</p>
              <ul class="bullet-list-tight">
                <li>Devolvam todos os dados utilizados aos seus respectivos donos.</li>
                <li>Virem a peça de território para o lado da <strong>"solução"</strong>.</li>
                <li>O jogador ativo lê a resolução científica em voz alta para toda a mesa.</li>
                <li>Recolham do Banco Geral os Marcadores de Financiamento correspondentes ao bônus indicado no verso da peça (+2, +4 ou +6) e adicionem-nos ao Banco Comunitário da equipe.</li>
                <li>O jogador responsável por colocar o último dado que resolveu o caso ganha o direito de comprar imediatamente <strong>1 nova carta de coleção</strong> para a sua mão (respeitando o limite máximo e os acervos permitidos pela sua prancheta).</li>
              </ul>
            </div>
          </div>
        </section>

        <!-- SECTION 7: ENDGAME -->
        <section id="endgame" class="doc-section">
          <div class="section-title-wrapper">
            <span class="section-num">07</span>
            <h2>Fim de Jogo</h2>
          </div>
          
          <div class="endgame-conditions">
            <div class="end-card victory">
              <div class="end-header">
                <span class="end-icon">🏆</span>
                <h3>Vitória da Equipe</h3>
              </div>
              <p>A equipe vence a partida imediatamente se conseguir erradicar todas as ameaças do mapa, virando todas as 12 peças de território para o lado da solução. Parabéns, doutores! A saúde pública foi defendida com sucesso!</p>
            </div>

            <div class="end-card defeat">
              <div class="end-header">
                <span class="end-icon">⚠️</span>
                <h3>Derrota da Equipe</h3>
              </div>
              <p>Os jogadores perdem a partida se o financiamento e os recursos de pesquisa esgotarem. Isso acontece se qualquer cientista precisar comprar uma carta e todos os 5 baralhos de Coleções Biológicas estiverem completamente vazios enquanto ainda houver algum território com o lado do "problema" virado para cima na mesa.</p>
            </div>
          </div>
        </section>

      </main>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from 'vue';

defineEmits(['go-home']);

const activeSection = ref('intro');

const sections = [
  'intro',
  'objective',
  'components',
  'setup',
  'professions',
  'gameplay',
  'endgame'
];

const printManual = () => {
  window.print();
};

const handleScroll = () => {
  const scrollPosition = window.scrollY + 120; // Offset for header
  
  for (const sectionId of sections) {
    const el = document.getElementById(sectionId);
    if (el) {
      const top = el.offsetTop;
      const height = el.offsetHeight;
      if (scrollPosition >= top && scrollPosition < top + height) {
        activeSection.value = sectionId;
        break;
      }
    }
  }
};

onMounted(() => {
  window.addEventListener('scroll', handleScroll);
  // Trigger once to set initial active section
  handleScroll();
});

onUnmounted(() => {
  window.removeEventListener('scroll', handleScroll);
});
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Cinzel:wght@600;800;900&family=Outfit:wght@300;400;500;600;700&display=swap');

/* Color Variables */
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

.manual-container {
  font-family: 'Outfit', sans-serif;
  background-color: #0f1115;
  color: #e2e8f0;
  min-height: 100vh;
  padding: 0;
  box-sizing: border-box;
}

/* Header */
.manual-header {
  position: sticky;
  top: 0;
  background: rgba(15, 17, 21, 0.95);
  backdrop-filter: blur(12px);
  border-bottom: 1px solid rgba(255, 255, 255, 0.08);
  padding: 16px 40px;
  display: flex;
  justify-content: space-between;
  align-items: center;
  z-index: 100;
}

.header-inner {
  display: flex;
  flex-direction: column;
  gap: 4px;
}

.btn-back {
  background: transparent;
  color: #94a3b8;
  border: 1px solid rgba(255, 255, 255, 0.08);
  border-radius: 8px;
  padding: 6px 12px;
  font-weight: 600;
  font-size: 12px;
  cursor: pointer;
  display: inline-flex;
  align-items: center;
  gap: 6px;
  transition: all 0.2s ease;
  width: fit-content;
}

.btn-back:hover {
  background: rgba(255, 255, 255, 0.05);
  color: #f8fafc;
}

.title-area h1 {
  font-family: 'Cinzel', serif;
  font-size: 22px;
  font-weight: 800;
  margin: 4px 0 2px 0;
  background: linear-gradient(90deg, #38bdf8, #a855f7);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
}

.brand-tag {
  font-size: 8px;
  font-weight: 700;
  letter-spacing: 1px;
  text-transform: uppercase;
  color: #38bdf8;
}

.subtitle {
  color: #94a3b8;
  font-size: 13px;
  margin: 0;
}

.btn-print {
  background: linear-gradient(135deg, #38bdf8 0%, #0284c7 100%);
  color: white;
  border: none;
  border-radius: 8px;
  padding: 10px 18px;
  font-weight: 600;
  font-size: 13px;
  cursor: pointer;
  display: flex;
  align-items: center;
  gap: 8px;
  transition: all 0.2s ease;
  box-shadow: 0 4px 12px rgba(56, 189, 248, 0.25);
}

.btn-print:hover {
  transform: translateY(-1px);
  box-shadow: 0 6px 16px rgba(56, 189, 248, 0.35);
}

/* Layout */
.manual-layout {
  display: flex;
  max-width: 1200px;
  margin: 0 auto;
  padding: 40px 20px;
  gap: 40px;
}

/* Sidebar Navigation */
.manual-sidebar {
  width: 260px;
  position: sticky;
  top: 100px;
  height: fit-content;
  background: rgba(22, 28, 38, 0.6);
  border: 1px solid rgba(255, 255, 255, 0.06);
  border-radius: 16px;
  padding: 20px;
  box-sizing: border-box;
}

.sidebar-header {
  font-family: 'Cinzel', serif;
  font-size: 13px;
  font-weight: 700;
  text-transform: uppercase;
  letter-spacing: 1px;
  color: #94a3b8;
  border-bottom: 1px solid rgba(255, 255, 255, 0.08);
  padding-bottom: 8px;
  margin-bottom: 15px;
}

.nav-links {
  list-style: none;
  padding: 0;
  margin: 0;
  display: flex;
  flex-direction: column;
  gap: 8px;
}

.nav-link {
  color: #94a3b8;
  text-decoration: none;
  font-size: 13.5px;
  padding: 8px 12px;
  border-radius: 8px;
  display: block;
  transition: all 0.2s ease;
}

.nav-link:hover {
  background: rgba(255, 255, 255, 0.03);
  color: #f1f5f9;
}

.nav-link.active {
  background: linear-gradient(90deg, rgba(56, 189, 248, 0.15), transparent);
  color: #38bdf8;
  font-weight: 600;
  border-left: 3px solid #38bdf8;
  padding-left: 9px;
}

/* Main Document Scroll */
.manual-document {
  flex-grow: 1;
  max-width: 860px;
  display: flex;
  flex-direction: column;
  gap: 60px;
}

.doc-section {
  background: rgba(22, 28, 38, 0.3);
  border: 1px solid rgba(255, 255, 255, 0.03);
  border-radius: 20px;
  padding: 30px;
  box-sizing: border-box;
  scroll-margin-top: 110px;
}

.section-title-wrapper {
  display: flex;
  align-items: center;
  gap: 15px;
  border-bottom: 1px solid rgba(255, 255, 255, 0.08);
  padding-bottom: 12px;
  margin-bottom: 20px;
}

.section-num {
  font-family: 'Cinzel', serif;
  font-size: 24px;
  font-weight: 800;
  color: #38bdf8;
  opacity: 0.6;
}

.section-title-wrapper h2 {
  font-family: 'Cinzel', serif;
  font-size: 22px;
  font-weight: 800;
  margin: 0;
  color: #f8fafc;
}

.lead-text {
  font-size: 15px;
  color: #94a3b8;
  font-style: italic;
  margin-top: -10px;
  margin-bottom: 24px;
}

p {
  font-size: 14.5px;
  line-height: 1.6;
  color: #cbd5e1;
  margin: 0 0 16px 0;
}

p:last-child {
  margin-bottom: 0;
}

/* Game Meta Cards */
.game-meta-cards {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 15px;
  margin-bottom: 24px;
}

.meta-card {
  background: rgba(255, 255, 255, 0.02);
  border: 1px solid rgba(255, 255, 255, 0.06);
  border-radius: 12px;
  padding: 15px;
  display: flex;
  flex-direction: column;
  align-items: center;
  text-align: center;
}

.meta-icon {
  font-size: 24px;
  margin-bottom: 8px;
}

.meta-label {
  font-size: 10px;
  text-transform: uppercase;
  color: #64748b;
  font-weight: 700;
  letter-spacing: 0.5px;
  margin-bottom: 4px;
}

.meta-val {
  font-size: 13px;
  color: #f1f5f9;
  font-weight: 600;
}

/* Dialog/Info Box */
.dialog-box {
  background: linear-gradient(135deg, rgba(56, 189, 248, 0.05) 0%, rgba(168, 85, 247, 0.05) 100%);
  border-left: 4px solid #38bdf8;
  border-radius: 0 12px 12px 0;
  padding: 20px;
  box-sizing: border-box;
}

.dialog-box p {
  margin: 0;
  font-size: 14px;
}

/* Components Grid */
.components-grid {
  display: grid;
  grid-template-columns: 1fr;
  gap: 20px;
}

.comp-item-card {
  background: rgba(255, 255, 255, 0.02);
  border: 1px solid rgba(255, 255, 255, 0.05);
  border-radius: 12px;
  padding: 20px;
}

.comp-item-card h3 {
  font-family: 'Cinzel', serif;
  font-size: 15px;
  margin: 0 0 10px 0;
  color: #f1f5f9;
}

.bullet-list {
  list-style: none;
  padding: 0;
  margin: 10px 0 0 0;
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 8px;
}

.bullet-list li {
  font-size: 13px;
  color: #cbd5e1;
  display: flex;
  align-items: center;
  gap: 8px;
}

.bullet-list li::before {
  content: '🧬';
  font-size: 10px;
}

/* Setup List */
.setup-list {
  list-style: none;
  padding: 0;
  margin: 0;
  display: flex;
  flex-direction: column;
  gap: 25px;
  position: relative;
}

.setup-list::before {
  content: '';
  position: absolute;
  top: 15px;
  left: 14px;
  bottom: 15px;
  width: 1px;
  background: rgba(255, 255, 255, 0.08);
}

.setup-list li {
  position: relative;
  padding-left: 45px;
}

.setup-list li::before {
  content: '';
  position: absolute;
  top: 12px;
  left: 10px;
  width: 9px;
  height: 9px;
  background: #38bdf8;
  border-radius: 50%;
  border: 3px solid #0f1115;
  z-index: 2;
  box-shadow: 0 0 10px #38bdf8;
}

.setup-step-header {
  font-family: 'Cinzel', serif;
  font-size: 15px;
  font-weight: 700;
  color: #f8fafc;
  margin-bottom: 6px;
}

/* Professions List */
.section-intro {
  margin-bottom: 24px;
  color: #94a3b8;
}

.professions-list {
  display: flex;
  flex-direction: column;
  gap: 24px;
}

.profession-card {
  border: 1px solid rgba(255, 255, 255, 0.05);
  border-radius: 16px;
  background: linear-gradient(135deg, rgba(255, 255, 255, 0.01) 0%, rgba(255, 255, 255, 0.03) 100%);
  overflow: hidden;
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.15);
  border-left: 4px solid var(--primary-color, #455a64);
}

.prof-header {
  background: rgba(0, 0, 0, 0.15);
  padding: 16px 20px;
  display: flex;
  align-items: center;
  gap: 15px;
}

.prof-icon {
  font-size: 28px;
}

.prof-header h3 {
  font-family: 'Cinzel', serif;
  font-size: 16px;
  margin: 0 0 2px 0;
  color: #ffffff;
}

.prof-label {
  font-size: 11px;
  color: var(--primary-color);
  font-weight: 600;
  text-transform: uppercase;
  letter-spacing: 0.5px;
}

.prof-details {
  padding: 20px;
}

.prof-stats {
  display: flex;
  gap: 20px;
  margin-bottom: 12px;
  background: rgba(255, 255, 255, 0.02);
  padding: 6px 14px;
  border-radius: 8px;
  width: fit-content;
  font-size: 13px;
  color: #f1f5f9;
  border: 1px solid rgba(255, 255, 255, 0.04);
}

.prof-text {
  font-size: 13.5px;
  margin: 0;
  line-height: 1.5;
}

/* Gameplay Steps */
.steps-container {
  display: flex;
  flex-direction: column;
  gap: 30px;
}

.game-step {
  position: relative;
  background: rgba(255, 255, 255, 0.01);
  border: 1px solid rgba(255, 255, 255, 0.04);
  border-radius: 16px;
  padding: 24px;
}

.step-badge {
  background: linear-gradient(135deg, #38bdf8 0%, #0284c7 100%);
  color: white;
  font-size: 9px;
  font-weight: 700;
  text-transform: uppercase;
  letter-spacing: 0.8px;
  padding: 4px 10px;
  border-radius: 9999px;
  width: fit-content;
  margin-bottom: 12px;
}

.game-step h3 {
  font-family: 'Cinzel', serif;
  font-size: 16px;
  margin: 0 0 10px 0;
  color: #ffffff;
}

/* Cost Progression Chart */
.cost-progression-box {
  display: flex;
  align-items: center;
  gap: 10px;
  background: rgba(0, 0, 0, 0.2);
  border: 1px solid rgba(255, 255, 255, 0.05);
  border-radius: 12px;
  padding: 15px;
  margin: 15px 0;
  justify-content: center;
  flex-wrap: wrap;
}

.cost-node {
  background: rgba(255, 255, 255, 0.03);
  border: 1px solid rgba(255, 255, 255, 0.08);
  border-radius: 8px;
  padding: 8px 16px;
  display: flex;
  flex-direction: column;
  align-items: center;
}

.cost-node .req {
  font-size: 9px;
  color: #64748b;
  text-transform: uppercase;
  font-weight: 700;
}

.cost-node .val {
  font-size: 14px;
  color: #a855f7;
  font-weight: 700;
}

.cost-arrow {
  color: #64748b;
  font-size: 16px;
}

.cost-note {
  font-size: 12px;
  color: #64748b;
  font-style: italic;
}

/* Rule Boxes */
.rules-highlight {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 20px;
  margin: 20px 0;
}

.rule-box {
  background: rgba(56, 189, 248, 0.02);
  border: 1px solid rgba(56, 189, 248, 0.1);
  border-radius: 12px;
  padding: 20px;
}

.rule-box h4 {
  font-family: 'Cinzel', serif;
  font-size: 13px;
  font-weight: 700;
  text-transform: uppercase;
  margin: 0 0 10px 0;
  color: #38bdf8;
  letter-spacing: 0.5px;
}

.rule-box p {
  margin: 0;
  font-size: 13px;
  line-height: 1.5;
}

.coringa-box {
  background: rgba(168, 85, 247, 0.04);
  border: 1px solid rgba(168, 85, 247, 0.12);
  border-radius: 12px;
  padding: 15px 20px;
  margin: 15px 0;
  font-size: 13.5px;
  line-height: 1.5;
  color: #e9d5ff;
}

.teamwork-box {
  background: rgba(22, 163, 74, 0.04);
  border: 1px solid rgba(22, 163, 74, 0.1);
  border-radius: 12px;
  padding: 15px 20px;
  margin: 15px 0;
  font-size: 13.5px;
  line-height: 1.5;
}

.bullet-list-tight {
  list-style: none;
  padding: 0;
  margin: 15px 0 0 0;
  display: flex;
  flex-direction: column;
  gap: 8px;
}

.bullet-list-tight li {
  font-size: 13.5px;
  color: #cbd5e1;
  display: flex;
  align-items: flex-start;
  gap: 10px;
  line-height: 1.5;
}

.bullet-list-tight li::before {
  content: '✔';
  color: #22c55e;
  font-weight: bold;
}

/* Endgame Conditions */
.endgame-conditions {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 25px;
  margin-top: 15px;
}

.end-card {
  border-radius: 16px;
  padding: 24px;
  box-sizing: border-box;
  border: 1px solid rgba(255, 255, 255, 0.04);
}

.end-card.victory {
  background: linear-gradient(135deg, rgba(22, 163, 74, 0.05) 0%, transparent 100%);
  border-left: 4px solid #22c55e;
}

.end-card.defeat {
  background: linear-gradient(135deg, rgba(220, 38, 38, 0.05) 0%, transparent 100%);
  border-left: 4px solid #dc2626;
}

.end-header {
  display: flex;
  align-items: center;
  gap: 12px;
  margin-bottom: 12px;
}

.end-icon {
  font-size: 24px;
}

.end-card h3 {
  font-family: 'Cinzel', serif;
  font-size: 16px;
  margin: 0;
  color: #ffffff;
}

.end-card p {
  margin: 0;
  font-size: 13px;
  line-height: 1.5;
}

/* ------------------------------------------------------------- */
/* RESPONSIVE DESIGN                                             */
/* ------------------------------------------------------------- */
@media (max-width: 900px) {
  .manual-layout {
    flex-direction: column;
    padding: 20px;
  }
  
  .manual-sidebar {
    width: 100%;
    position: static;
    margin-bottom: 20px;
  }
  
  .game-meta-cards, 
  .rules-highlight, 
  .endgame-conditions {
    grid-template-columns: 1fr;
  }

  .bullet-list {
    grid-template-columns: 1fr;
  }
  
  .manual-header {
    flex-direction: column;
    align-items: flex-start;
    gap: 15px;
    padding: 15px 20px;
  }
  
  .header-actions {
    width: 100%;
  }
  
  .btn-print {
    width: 100%;
    justify-content: center;
  }
}

/* ------------------------------------------------------------- */
/* PRINT STYLESHEET                                              */
/* ------------------------------------------------------------- */
@media print {
  html, body {
    background: #ffffff !important;
    color: #000000 !important;
    font-size: 11pt !important;
  }
  
  .manual-container {
    background: transparent !important;
    color: #000000 !important;
    padding: 0 !important;
  }
  
  .no-print {
    display: none !important;
  }
  
  .manual-layout {
    display: block !important;
    padding: 0 !important;
    margin: 0 !important;
  }
  
  .manual-document {
    max-width: 100% !important;
    display: block !important;
  }
  
  .doc-section {
    background: transparent !important;
    border: none !important;
    padding: 10mm 0 !important;
    margin-bottom: 20mm !important;
    page-break-inside: avoid !important;
    break-inside: avoid !important;
  }
  
  .section-title-wrapper h2 {
    color: #000000 !important;
    font-size: 18pt !important;
  }
  
  p, li, .prof-text, .end-card p {
    color: #1a1a1a !important;
    font-size: 10pt !important;
    line-height: 1.5 !important;
  }
  
  .meta-card, .comp-item-card, .profession-card, .game-step, .cost-progression-box, .rule-box, .coringa-box, .teamwork-box, .end-card {
    background: transparent !important;
    border: 0.2mm solid #cccccc !important;
    color: #000000 !important;
    box-shadow: none !important;
  }
  
  .prof-header {
    background: #f1f5f9 !important;
    border-bottom: 0.2mm solid #cccccc !important;
  }
  
  .prof-header h3, .prof-label {
    color: #000000 !important;
  }

  .prof-stats {
    background: transparent !important;
    border: 0.2mm solid #dddddd !important;
    color: #000000 !important;
  }
  
  .step-badge {
    background: #000000 !important;
    color: #ffffff !important;
  }
}
</style>
