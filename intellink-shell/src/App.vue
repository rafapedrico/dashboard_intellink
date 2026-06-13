<template>
  <!-- SE ESTIVER NO MODO TELA CHEIA, RENDERIZA APENAS O IFRAME PURO NA RAIZ -->
  <div v-if="isFullScreen" class="full-screen-container-absolute">
    <iframe :src="currentIframeUrl" class="pure-fullscreen-iframe"></iframe>
  </div>

  <!-- LAYOUT NORMAL DO DASHBOARD (SÓ APARECE SE NÃO ESTIVER EM TELA CHEIA) -->
  <div v-else class="intellink-app">
    <div v-if="!isLoggedIn" class="login-view">
      <div class="login-glass-card">
        <div class="login-header-block">
          <div class="login-logo-placeholder">
            <i class="fa-solid fa-layer-group logo-icon-glow"></i>
          </div>
          <h1 class="login-title">INTELLINK</h1>
          <p class="login-tagline">INTEGRAÇÃO. INTELIGÊNCIA. RESULTADOS.</p>
          <p class="login-welcome-text">Acesse o sistema como Administrador Master</p>
        </div>

        <form @submit.prevent="handleLogin" class="login-form-modern">
          
          <div class="input-group-modern">
            <span class="input-icon-wrapper"><i class="fa-solid fa-industry"></i></span>
            <input type="text" placeholder="Licença / Unidade" required value="BR-SOROCABA-JATOBA" class="modern-field" />
          </div>

          <div class="input-group-modern">
            <span class="input-icon-wrapper"><i class="fa-solid fa-user"></i></span>
            <input type="text" placeholder="Usuário" required value="admin.master" class="modern-field" />
          </div>

          <div class="input-group-modern">
            <span class="input-icon-wrapper"><i class="fa-solid fa-lock"></i></span>
            <input type="password" placeholder="Senha" required value="********" class="modern-field" />
            <span class="password-toggle-wrapper"><i class="fa-regular fa-eye"></i></span>
          </div>

          <div class="login-options-row">
            <label class="remember-me-label">
              <input type="checkbox" checked class="modern-checkbox" /> Lembrar de mim
            </label>
            <a href="#" class="forgot-password-link" @click.prevent>Esqueci minha senha</a>
          </div>

          <button type="submit" class="btn-login-modern">ENTRAR</button>
          
          <div class="divider-text-row">
            <span>ou</span>
          </div>

          <button type="button" class="btn-2fa-modern" @click.prevent>
            <i class="fa-solid fa-shield-halved"></i> Acessar com autenticação de dois fatores
          </button>
        </form>

        <footer class="login-footer-secure">
          <i class="fa-solid fa-lock-open"></i> Sistema seguro e criptografado
        </footer>
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
          <div class="nav-button" @click="entrarModoCadastro">
            <img src="./assets/dois.png" alt="Cadastros">
          </div>
    <div class="nav-button" @click="entrarModoPCP">
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
          <iframe :src="currentIframeUrl" frameborder="0" class="subsystem-iframe"></iframe>
        </div>

        <div v-else-if="currentModule === 'pcp'" class="subsystem-container">
          <div class="iframe-header"><span>📅 Módulo: PCP Ativo</span><button @click="currentModule = null" class="btn-close-sub">Voltar</button></div>
          <iframe :src="currentIframeUrl" frameborder="0" class="subsystem-iframe"></iframe>
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
import { ref, computed, onMounted } from 'vue'

const isLoggedIn = ref(false)
const currentModule = ref(null)
const currentTime = ref('')
const currentIframeUrl = ref('./assets/principal.png')

// Computada que deriva o fullscreen do módulo ativo (unifica Qualidade, PCP e Cadastro)
const isFullScreen = computed(() => {
  return currentModule.value === 'qualidade' || currentModule.value === 'pcp' || currentModule.value === 'cadastro'
})

const handleLogin = () => { isLoggedIn.value = true }

const handleModuleClick = (moduleName) => {
  currentModule.value = moduleName
}

const entrarModoQualidade = () => {
  currentModule.value = 'qualidade'
  currentIframeUrl.value = 'http://localhost:5005'
}

const entrarModoCadastro = () => {
  currentModule.value = 'cadastro'
  currentIframeUrl.value = 'http://localhost:5002'
}

const entrarModoPCP = () => {
  currentModule.value = 'pcp'
  currentIframeUrl.value = 'http://localhost:5003'
}

const sairModoFullscreen = () => {
  currentModule.value = null
  currentIframeUrl.value = './assets/principal.png'
}

const updateTime = () => {
  const now = new Date()
  currentTime.value = now.toLocaleTimeString('pt-BR')
}

onMounted(() => {
  setInterval(updateTime, 1000)
  updateTime()

  // Escuta mensagens do iframe (botão VOLTAR interno) — Qualidade e PCP
  window.addEventListener('message', (event) => {
    if (event.data === 'fechar-qualidade' || event.data === 'fechar-pcp') {
      sairModoFullscreen()
    }
  })
})
</script>

<style>
/* ============================================
   CSS GLOBAL — sem scoped, sem data-v-*
   Todas as regras são aplicadas diretamente
   ============================================ */

/* RESET BASE */
html, body {
  margin: 0 !important;
  padding: 0 !important;
  width: 100vw !important;
  height: 100vh !important;
  overflow: hidden !important;
  background-color: #020617;
}

/* ===== LOGIN — CONTAINER PRINCIPAL ===== */
.login-view {
  width: 100vw !important;
  height: 100vh !important;
  display: flex !important;
  align-items: center !important;
  justify-content: center !important;
  background-image: linear-gradient(rgba(11, 15, 25, 0.45), rgba(11, 15, 25, 0.45)), url('/fundo-login.png') !important;
  background-size: cover !important;
  background-position: center !important;
  background-repeat: no-repeat !important;
  margin: 0 !important;
  padding: 0 !important;
}

/* ===== CARD GLASSMORPHISM ===== */
.login-glass-card {
  width: 480px !important;
  background: rgba(15, 23, 42, 0.45) !important;
  backdrop-filter: blur(20px) !important;
  -webkit-backdrop-filter: blur(20px) !important;
  border: 1px solid rgba(255, 255, 255, 0.08) !important;
  border-radius: 16px !important;
  padding: 40px 35px !important;
  box-shadow: 0 20px 50px rgba(0, 0, 0, 0.4) !important;
  box-sizing: border-box !important;
  display: flex !important;
  flex-direction: column !important;
}

/* ===== HEADER DO LOGIN ===== */
.login-header-block {
  text-align: center !important;
  margin-bottom: 25px !important;
}
.login-logo-placeholder {
  font-size: 2.2rem !important;
  margin-bottom: 10px !important;
}
.logo-icon-glow {
  color: #3b82f6 !important;
  filter: drop-shadow(0 0 10px rgba(59, 130, 246, 0.6)) !important;
}
.login-title {
  font-size: 2.2rem !important;
  font-weight: 800 !important;
  color: #ffffff !important;
  letter-spacing: 2px !important;
  margin: 0 0 4px 0 !important;
}
.login-tagline {
  color: #0284c7 !important;
  font-size: 0.75rem !important;
  font-weight: 700 !important;
  letter-spacing: 1px !important;
  margin: 0 0 20px 0 !important;
}
.login-welcome-text {
  color: #94a3b8 !important;
  font-size: 0.9rem !important;
  margin: 0 !important;
}

/* ===== GRUPOS DE INPUT MODERNOS ===== */
.login-form-modern {
  display: flex !important;
  flex-direction: column !important;
  gap: 16px !important;
}
.input-group-modern {
  position: relative !important;
  display: flex !important;
  align-items: center !important;
}
.input-icon-wrapper {
  position: absolute !important;
  left: 14px !important;
  color: #3b82f6 !important;
  font-size: 1.1rem !important;
}
.password-toggle-wrapper {
  position: absolute !important;
  right: 14px !important;
  color: #64748b !important;
  cursor: pointer !important;
}
.modern-field {
  width: 100% !important;
  background: rgba(15, 23, 42, 0.6) !important;
  border: 1px solid rgba(255, 255, 255, 0.1) !important;
  border-radius: 8px !important;
  padding: 14px 45px !important;
  color: #ffffff !important;
  font-size: 0.95rem !important;
  outline: none !important;
  transition: all 0.2s ease !important;
}
.modern-field:focus {
  border-color: #3b82f6 !important;
  box-shadow: 0 0 0 2px rgba(59, 130, 246, 0.25) !important;
  background: rgba(15, 23, 42, 0.8) !important;
}

/* ===== OPÇÕES ===== */
.login-options-row {
  display: flex !important;
  justify-content: space-between !important;
  align-items: center !important;
  font-size: 0.85rem !important;
  margin-top: 2px !important;
}
.remember-me-label {
  color: #94a3b8 !important;
  display: flex !important;
  align-items: center !important;
  gap: 8px !important;
  cursor: pointer !important;
}
.modern-checkbox {
  accent-color: #3b82f6 !important;
}
.forgot-password-link {
  color: #3b82f6 !important;
  text-decoration: none !important;
  font-weight: 500 !important;
}
.forgot-password-link:hover {
  text-decoration: underline !important;
}

/* ===== BOTÕES ===== */
.btn-login-modern {
  width: 100% !important;
  background: linear-gradient(90deg, #1d4ed8 0%, #2563eb 100%) !important;
  color: #ffffff !important;
  border: none !important;
  border-radius: 8px !important;
  padding: 14px !important;
  font-size: 1rem !important;
  font-weight: 700 !important;
  cursor: pointer !important;
  box-shadow: 0 4px 15px rgba(37, 99, 235, 0.3) !important;
  transition: all 0.2s ease !important;
  letter-spacing: 0.5px !important;
}
.btn-login-modern:hover {
  transform: translateY(-1px) !important;
  box-shadow: 0 6px 20px rgba(37, 99, 235, 0.4) !important;
}

.divider-text-row {
  text-align: center !important;
  position: relative !important;
  margin: 4px 0 !important;
}
.divider-text-row::before,
.divider-text-row::after {
  content: "" !important;
  position: absolute !important;
  top: 50% !important;
  width: 42% !important;
  height: 1px !important;
  background: rgba(255, 255, 255, 0.1) !important;
}
.divider-text-row::before { left: 0 !important; }
.divider-text-row::after { right: 0 !important; }
.divider-text-row span {
  font-size: 0.85rem !important;
  color: #64748b !important;
  background: transparent !important;
  padding: 0 10px !important;
}

.btn-2fa-modern {
  width: 100% !important;
  background: transparent !important;
  color: #e2e8f0 !important;
  border: 1px solid rgba(255, 255, 255, 0.15) !important;
  border-radius: 8px !important;
  padding: 12px !important;
  font-size: 0.85rem !important;
  font-weight: 500 !important;
  cursor: pointer !important;
  display: flex !important;
  align-items: center !important;
  justify-content: center !important;
  gap: 8px !important;
  transition: all 0.2s ease !important;
}
.btn-2fa-modern:hover {
  background: rgba(255, 255, 255, 0.05) !important;
  border-color: rgba(255, 255, 255, 0.3) !important;
}

/* ===== FOOTER SEGURO ===== */
.login-footer-secure {
  text-align: center !important;
  margin-top: 30px !important;
  font-size: 0.8rem !important;
  color: #475569 !important;
  display: flex !important;
  align-items: center !important;
  justify-content: center !important;
  gap: 6px !important;
}

/* ===== FULLSCREEN IFRAME ===== */
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

/* ===== DASHBOARD LAYOUT ===== */
.intellink-app {
  width: 100vw !important;
  height: 100vh !important;
  background: #020617 !important;
  overflow: hidden !important;
  color: white !important;
  display: flex !important;
  flex-direction: column !important;
}
.dashboard-cockpit {
  flex: 1 !important;
  display: flex !important;
  flex-direction: column !important;
  overflow: hidden !important;
}
.top-control-bar {
  display: flex !important;
  justify-content: space-between !important;
  align-items: center !important;
  padding: 8px 20px !important;
  background: #111827 !important;
  border-bottom: 1px solid rgba(255, 255, 255, 0.1) !important;
  height: 50px !important;
}
.btn-exit, .btn-switch {
  background: rgba(255,255,255,0.05) !important;
  border: 1px solid #334155 !important;
  color: white !important;
  padding: 6px 14px !important;
  border-radius: 4px !important;
  cursor: pointer !important;
  font-weight: bold !important;
}
.btn-exit:hover { background: #ef4444 !important; }

.modules-grid {
  display: grid !important;
  grid-template-columns: repeat(4, 1fr) !important;
  padding: 0 !important;
  margin: 0 !important;
  background: #000 !important;
  width: 100vw !important;
  gap: 0 !important;
}
.nav-button {
  cursor: pointer !important;
  display: flex !important;
  width: 100% !important;
  height: 143px !important;
  margin: 0 !important;
  padding: 0 !important;
  background: transparent !important;
  border: none !important;
  outline: none !important;
  overflow: hidden !important;
}
.nav-button img {
  width: 100% !important;
  height: 143px !important;
  object-fit: fill !important;
  display: block !important;
}
.nav-button:hover img { filter: brightness(1.2) !important; }
.nav-button:active img { filter: brightness(0.9) !important; }

.main-display {
  flex: 1 !important;
  width: 100vw !important;
  position: relative !important;
  overflow: hidden !important;
  background: #020617 !important;
}
.full-bg-image {
  width: 100% !important;
  height: 100% !important;
  background-image: url('./assets/principal.png') !important;
  background-size: cover !important;
  background-position: center center !important;
  background-repeat: no-repeat !important;
}

/* ===== STORE ===== */
.store-container {
  width: 100vw !important;
  height: 100% !important;
  padding: 30px !important;
  background: #020617 !important;
  overflow-y: auto !important;
  display: flex !important;
  flex-direction: column !important;
}
.store-header {
  display: flex !important;
  justify-content: space-between !important;
  align-items: center !important;
  border-bottom: 1px solid rgba(240,246,252,0.1) !important;
  padding-bottom: 15px !important;
}
.store-subtitle {
  margin: 15px 0 !important;
  color: #8b949e !important;
  font-size: 1rem !important;
}
.btn-close-store {
  background: #3b82f6 !important;
  color: white !important;
  border: none !important;
  padding: 8px 16px !important;
  border-radius: 4px !important;
  cursor: pointer !important;
  font-weight: bold !important;
}
.store-grid {
  display: grid !important;
  grid-template-columns: repeat(auto-fit, minmax(200px, 1fr)) !important;
  gap: 20px !important;
  padding: 20px 0 !important;
}
.store-card {
  background: #0d1117 !important;
  border: 1px solid rgba(240,246,252,0.1) !important;
  border-radius: 8px !important;
  padding: 15px !important;
  display: flex !important;
  flex-direction: column !important;
  align-items: center !important;
  gap: 15px !important;
}
.store-card img {
  width: 100% !important;
  height: auto !important;
  object-fit: contain !important;
}
.btn-buy {
  width: 100% !important;
  padding: 10px !important;
  background: transparent !important;
  border: 1px solid #0078ff !important;
  color: #0078ff !important;
  border-radius: 4px !important;
  font-weight: bold !important;
  cursor: pointer !important;
  transition: all 0.2s !important;
}
.btn-buy:hover { background: #0078ff !important; color: white !important; }

/* ===== SUBSYSTEM (Cadastro e PCP) ===== */
.subsystem-container {
  width: 100vw !important;
  height: 100% !important;
  display: flex !important;
  flex-direction: column !important;
  background: #020617 !important;
}
.iframe-header {
  background: #0d1117 !important;
  border-bottom: 1px solid rgba(240, 246, 252, 0.1) !important;
  padding: 10px 20px !important;
  display: flex !important;
  justify-content: space-between !important;
  align-items: center !important;
}
.btn-close-sub {
  background: #ef4444 !important;
  color: white !important;
  border: none !important;
  padding: 6px 14px !important;
  border-radius: 4px !important;
  cursor: pointer !important;
  font-weight: bold !important;
}
.subsystem-iframe {
  flex: 1 !important;
  width: 100vw !important;
  height: 100% !important;
  border: none !important;
  background: transparent !important;
}
</style>