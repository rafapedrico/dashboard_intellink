<template>
  <!-- SE ESTIVER NO MODO TELA CHEIA, RENDERIZA APENAS O IFRAME PURO NA RAIZ -->
  <div v-if="isFullScreen" class="full-screen-container-absolute">
    <iframe :src="currentIframeUrl" class="pure-fullscreen-iframe"></iframe>
  </div>

  <!-- LAYOUT NORMAL DO DASHBOARD (SÓ APARECE SE NÃO ESTIVER EM TELA CHEIA) -->
  <div v-else class="intellink-app">
    <div v-if="!isLoggedIn" class="login-view">
      <div class="login-glass-card">
        <h1>INTELLINK<span>.</span></h1>
        <p>Portal de Gestão Industrial & Licenciamento</p>
        <form @submit.prevent="handleLogin">
          <input type="text" placeholder="Licença" required value="BR-SOROCABA-JATOBA">
          <input type="text" placeholder="Usuário" required value="admin.master">
          <input type="password" placeholder="Senha" required value="••••••••">
          <button type="submit" class="btn-login">Aprovar & Acessar Dashboard</button>
        </form>
      </div>
    </div>

    <div v-else class="dashboard-cockpit">
      <div>
        <header class="top-control-bar">
          <div class="top-left">
            <button @click="isLoggedIn = false" class="btn-exit"><i class="fa-solid fa-arrow-left"></i> SAIR</button>
          </div>
          <div class="top-center">
            <span class="user-info"><i class="fa-solid fa-user"></i> Usuário logado: <strong>RAFAEL PEDRICO</strong></span>
            <button class="btn-switch"><i class="fa-solid fa-rotate"></i> TROCAR DE USUÁRIO</button>
          </div>
          <div class="top-right">
            <div class="date-time">
              <span class="date"><i class="fa-solid fa-calendar-days"></i> 28/05/2025</span>
              <span class="clock"><i class="fa-solid fa-clock"></i> {{ currentTime }}</span>
            </div>
          </div>
        </header>

        <nav class="modules-grid">
          <div class="nav-button" @click="handleModuleClick('cadastro')">
            <img src="./assets/dois.png" alt="Cadastros">
          </div>
          <div class="nav-button" @click="handleModuleClick('pcp')">
            <img src="./assets/quatro.png" alt="PCP">
          </div>
          <div class="nav-button" @click="entrarModoQualidade">
            <img src="./assets/cinco.png" alt="Controle de Qualidade">
          </div>
          <div class="nav-button" @click="handleModuleClick('store')">
            <img src="./assets/onze.png" alt="INTELLINK Licença/Store">
          </div>
        </nav>
      </div>

      <main class="main-display">
        <div v-if="currentModule === null" class="full-bg-image"></div>

        <div v-else-if="currentModule === 'cadastro'" class="subsystem-container">
          <div class="iframe-header"><span>📦 Módulo: Cadastros Ativo</span><button @click="currentModule = null" class="btn-close-sub">Voltar</button></div>
          <iframe src="http://localhost:5002" frameborder="0" class="subsystem-iframe"></iframe>
        </div>

        <div v-else-if="currentModule === 'pcp'" class="subsystem-container">
          <div class="iframe-header"><span>📅 Módulo: PCP Ativo</span><button @click="currentModule = null" class="btn-close-sub">Voltar</button></div>
          <iframe src="http://localhost:5003" frameborder="0" class="subsystem-iframe"></iframe>
        </div>

        <div v-else-if="currentModule === 'store'" class="store-container">
          <div class="store-header">
            <h2>🛒 INTELLINK Store — Upgrade de Licença</h2>
            <button @click="currentModule = null" class="btn-close-store">Voltar ao Painel</button>
          </div>
          <p class="store-subtitle">Expanda a capacidade da sua fábrica ativando novos módulos modulares:</p>
          <div class="store-grid">
            <div class="store-card"><img src="./assets/tres.png" alt="Linha de Produção"><button class="btn-buy">Solicitar Ativação</button></div>
            <div class="store-card"><img src="./assets/um.png" alt="Login/Permissões"><button class="btn-buy">Solicitar Ativação</button></div>
            <div class="store-card"><img src="./assets/seis.png" alt="Monitoramento Real-Time"><button class="btn-buy">Solicitar Ativação</button></div>
            <div class="store-card"><img src="./assets/sete.png" alt="Recebimento/Expedição"><button class="btn-buy">Solicitar Ativação</button></div>
            <div class="store-card"><img src="./assets/oito.png" alt="Relatórios"><button class="btn-buy">Solicitar Ativação</button></div>
            <div class="store-card"><img src="./assets/nove.png" alt="Rastreabilidade"><button class="btn-buy">Solicitar Ativação</button></div>
            <div class="store-card"><img src="./assets/dez.png" alt="Backup/Segurança"><button class="btn-buy">Solicitar Ativação</button></div>
          </div>
        </div>
      </main>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'

const isLoggedIn = ref(false)
const currentModule = ref(null)
const currentTime = ref('')
const isFullScreen = ref(false)
const currentIframeUrl = ref('./assets/principal.png')

const handleLogin = () => { isLoggedIn.value = true }

const handleModuleClick = (moduleName) => {
  currentModule.value = moduleName
}

const entrarModoQualidade = () => {
  currentIframeUrl.value = 'http://localhost:5173'
  isFullScreen.value = true
  currentModule.value = null
}

const sairModoQualidade = () => {
  currentIframeUrl.value = './assets/principal.png'
  isFullScreen.value = false
}

const updateTime = () => {
  const now = new Date()
  currentTime.value = now.toLocaleTimeString('pt-BR')
}

onMounted(() => {
  setInterval(updateTime, 1000)
  updateTime()

  // Escuta mensagens do iframe da Qualidade (botão VOLTAR interno)
  window.addEventListener('message', (event) => {
    if (event.data === 'fechar-qualidade') {
      sairModoQualidade()
    }
  })
})
</script>

<style>
html, body {
  margin: 0 !important;
  padding: 0 !important;
  width: 100vw !important;
  height: 100vh !important;
  overflow: hidden !important;
  background-color: #020617;
}
</style>

<style scoped>
/* Remove qualquer elemento do caminho e força o container a ocupar do pixel 0 ao último pixel da tela */
.full-screen-container-absolute {
  position: fixed !important;
  top: 0 !important;
  left: 0 !important;
  width: 100vw !important;
  height: 100vh !important;
  margin: 0 !important;
  padding: 0 !important;
  background: #0b0f19 !important;
  z-index: 99999 !important;
  overflow: hidden !important;
}

/* Força o iframe a não aceitar nenhuma margem ou empurrão lateral */
.pure-fullscreen-iframe {
  width: 100vw !important;
  height: 100vh !important;
  border: none !important;
  margin: 0 !important;
  padding: 0 !important;
  display: block !important;
  position: absolute !important;
  top: 0 !important;
  left: 0 !important;
}

.intellink-app { width: 100vw; height: 100vh; background: #020617; overflow: hidden; color: white; display: flex; flex-direction: column; }
.login-view { height: 100%; display: flex; align-items: center; justify-content: center; background: #020617; }
.login-glass-card { background: rgba(15,20,30,0.5); backdrop-filter: blur(16px); padding: 40px; border-radius: 15px; border: 1px solid rgba(0, 120, 255, 0.3); text-align: center; }
.login-glass-card input { display: block; width: 100%; margin: 10px 0; padding: 12px; background: rgba(0,0,0,0.4); border: 1px solid rgba(240,246,252,0.1); color: white; border-radius: 6px; }
.btn-login { width: 100%; padding: 12px; background: #0078ff; border: none; border-radius: 6px; color: white; font-weight: bold; cursor: pointer; }

.dashboard-cockpit { flex: 1; display: flex; flex-direction: column; overflow: hidden; }
.top-control-bar { display: flex; justify-content: space-between; align-items: center; padding: 8px 20px; background: #111827; border-bottom: 1px solid rgba(255, 255, 255, 0.1); height: 50px; }
.btn-exit, .btn-switch { background: rgba(255,255,255,0.05); border: 1px solid #334155; color: white; padding: 6px 14px; border-radius: 4px; cursor: pointer; font-weight: bold; }
.btn-exit:hover { background: #ef4444; }

.modules-grid { 
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  padding: 0;
  margin: 0;
  background: #000;
  width: 100vw;
  gap: 0;
}
.nav-button { 
  cursor: pointer; 
  display: flex;
  width: 100%;
  height: 143px;
  margin: 0;
  padding: 0;
  background: transparent;
  border: none;
  outline: none;
  overflow: hidden;
}

.nav-button img { 
  width: 100%;
  height: 143px;
  object-fit: fill;
  display: block;
}

.nav-button:hover img {
  filter: brightness(1.2);
}

.nav-button:active img {
  filter: brightness(0.9);
}

.main-display { 
  flex: 1; 
  width: 100vw;
  position: relative;
  overflow: hidden;
  background: #020617;
}

.full-bg-image {
  width: 100%;
  height: 100%;
  background-image: url('./assets/principal.png');
  background-size: cover;
  background-position: center center;
  background-repeat: no-repeat;
}

/* SEÇÃO DA STORE */
.store-container { width: 100vw; height: 100%; padding: 30px; background: #020617; overflow-y: auto; display: flex; flex-direction: column; }
.store-header { display: flex; justify-content: space-between; align-items: center; border-bottom: 1px solid rgba(240,246,252,0.1); padding-bottom: 15px; }
.store-subtitle { margin: 15px 0; color: #8b949e; font-size: 1rem; }
.btn-close-store { background: #3b82f6; color: white; border: none; padding: 8px 16px; border-radius: 4px; cursor: pointer; font-weight: bold; }
.store-grid { display: grid; grid-template-columns: repeat(auto-fit, minmax(200px, 1fr)); gap: 20px; padding: 20px 0; }
.store-card { background: #0d1117; border: 1px solid rgba(240,246,252,0.1); border-radius: 8px; padding: 15px; display: flex; flex-direction: column; align-items: center; gap: 15px; }
.store-card img { width: 100%; height: auto; object-fit: contain; }
.btn-buy { width: 100%; padding: 10px; background: transparent; border: 1px solid #0078ff; color: #0078ff; border-radius: 4px; font-weight: bold; cursor: pointer; transition: all 0.2s; }
.btn-buy:hover { background: #0078ff; color: white; }

/* SUBSYSTEM (Cadastro e PCP em modo normal) */
.subsystem-container { width: 100vw; height: 100%; display: flex; flex-direction: column; background: #020617; }
.iframe-header { background: #0d1117; border-bottom: 1px solid rgba(240, 246, 252, 0.1); padding: 10px 20px; display: flex; justify-content: space-between; align-items: center; }
.btn-close-sub { background: #ef4444; color: white; border: none; padding: 6px 14px; border-radius: 4px; cursor: pointer; font-weight: bold; }
.subsystem-iframe { flex: 1; width: 100vw; height: 100%; border: none; background: transparent; }
</style>