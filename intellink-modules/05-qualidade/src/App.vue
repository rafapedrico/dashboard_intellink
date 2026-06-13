<template>
  <div class="quality-dashboard">
    <!-- CABEÇALHO INTERNO AJUSTADO -->
    <header class="quality-header">
      <div class="header-left">
        <!-- Evento para avisar o Shell que o usuário quer voltar -->
        <button class="btn-voltar-interno" @click="emitVoltar">
          <i class="fa-solid fa-arrow-left"></i> VOLTAR
        </button>
      </div>
      
      <!-- FRASE CENTRALIZADA PERFEITAMENTE -->
      <div class="header-center">
        <span class="brand">INTELLINK</span>
        <span class="divider">|</span>
        <span class="subtitle">DASHBOARD DA QUALIDADE</span>
      </div>
      
      <div class="header-right">
        <span class="user-meta"><i class="fa-solid fa-user-shield"></i> Supervisor de Qualidade</span>
      </div>
    </header>

    <!-- BARRA INTERNA DE SELEÇÃO DE 8 BOTÕES (OCUPANDO 100% DA LARGURA) -->
    <nav class="quality-nav">
      <button 
        v-for="tab in tabs" 
        :key="tab.id" 
        :class="['nav-item', { active: currentTab === tab.id }]"
        @click="currentTab = tab.id"
      >
        <span class="nav-icon">{{ tab.icon }}</span>
        <span class="nav-label">{{ tab.label }}</span>
      </button>
    </nav>

    <!-- ÁREA DE EXIBIÇÃO DA FIGURA - 100% DA TELA SEM MARGENS -->
    <main class="quality-display">
      <div class="image-full-screen-container">
        <div 
          class="current-module-view-bg" 
          :style="{ backgroundImage: `url(http://localhost:5005/${currentTab}.png)` }"
        ></div>
      </div>
    </main>
  </div>
</template>

<script setup>
import { ref } from 'vue'

const currentTab = ref('dashboard')

const tabs = ref([
  { id: 'dashboard', label: 'DASHBOARD', icon: '📊' },
  { id: 'plano', label: 'PLANO DE INSPEÇÃO', icon: '📋' },
  { id: 'dimensional', label: 'INSPEÇÃO DIMENSIONAL', icon: '📐' },
  { id: 'peca', label: 'PRIMEIRA PEÇA', icon: '⚙️' },
  { id: 'rnc', label: 'NÃO CONFORMIDADES', icon: '⚠️' },
  { id: 'instrumentos', label: 'INSTRUMENTOS', icon: '🔬' },
  { id: 'relatorios', label: 'RELATÓRIOS', icon: '📈' },
  { id: 'rastreabilidade', label: 'RASTREABILIDADE', icon: '🔍' }
])

// Emite uma mensagem para a janela pai (Shell) solicitando o fechamento da tela cheia
const emitVoltar = () => {
  window.parent.postMessage('fechar-qualidade', '*')
}
</script>

<style>
#app {
  width: 100vw !important;
  max-width: 100vw !important;
  min-height: 100vh !important;
  margin: 0 !important;
  padding: 0 !important;
  text-align: left !important;
  display: block !important;
}

html, body {
  margin: 0 !important;
  padding: 0 !important;
  width: 100vw !important;
  height: 100vh !important;
  overflow: hidden !important; /* Remove as barras de rolagem de dentro do iframe */
  background: #0b0f19;
}

.quality-dashboard {
  display: flex;
  flex-direction: column;
  height: 100vh;
  width: 100vw;
  background: #0b0f19;
  margin: 0 !important;
  padding: 0 !important;
  box-sizing: border-box;
}

/* Header com distribuição em 3 colunas para garantir a centralização real */
.quality-header { 
  display: grid; 
  grid-template-columns: 120px 1fr 120px; /* Dá um tamanho fixo para as pontas e deixa o centro expandir */
  align-items: center; 
  padding: 10px 20px; 
  background: #090d16; 
  border-bottom: 1px solid rgba(255,255,255,0.05); 
  height: 50px;
  box-sizing: border-box;
}

.header-left { display: flex; justify-content: flex-start; }
.header-center { display: flex; justify-content: center; align-items: center; gap: 8px; }
.header-right { display: flex; justify-content: flex-end; }

.brand { font-weight: 800; letter-spacing: 1px; color: #ffffff; font-size: 1.1rem; }
.divider { color: #334155; }
.subtitle { font-size: 0.9rem; font-weight: 600; color: #94a3b8; letter-spacing: 0.5px; }
.user-meta { font-size: 0.85rem; color: #64748b; }

/* Botão Voltar integrado de forma limpa no menu */
.btn-voltar-interno {
  background: linear-gradient(180deg, #1e293b 0%, #0f172a 100%);
  color: #f8fafc;
  border: 1px solid #3b82f6;
  padding: 6px 14px;
  font-size: 0.8rem;
  font-weight: 700;
  border-radius: 4px;
  cursor: pointer;
  box-shadow: 0 2px 5px rgba(0,0,0,0.3);
  transition: all 0.2s;
}
.btn-voltar-interno:hover { filter: brightness(1.2); border-color: #60a5fa; }

/* Garante que as 8 colunas ocupem exatamente a largura sem vazar 1 pixel sequer */
.quality-nav {
  display: grid;
  grid-template-columns: repeat(8, 1fr);
  background: #1e293b;
  border-bottom: 2px solid #0284c7;
  width: 100% !important;
  margin: 0 !important;
  padding: 0 !important;
  box-sizing: border-box;
}
.nav-item { background: transparent; border: none; border-right: 1px solid rgba(255,255,255,0.05); color: #94a3b8; padding: 12px 4px; display: flex; flex-direction: column; align-items: center; gap: 6px; cursor: pointer; transition: all 0.2s ease; outline: none; width: 100%; }
.nav-icon { font-size: 1.2rem; }
.nav-label { font-size: 0.65rem; font-weight: 700; text-align: center; }

.nav-item:hover { background: rgba(255,255,255,0.02); color: #ffffff; }
.nav-item.active { background: #0284c7; color: #ffffff; }

/* Área inferior TOTALMENTE LIMPA de margens e preenchimentos */
.quality-display { flex: 1; background: #0b0f19; margin: 0; padding: 0; width: 100vw; height: calc(100vh - 100px); overflow: hidden; display: flex; }
.image-full-screen-container { width: 100%; height: 100%; display: flex; margin: 0; padding: 0; overflow: hidden; }

.current-module-view-bg {
  width: 100%;
  height: 100%;
  background-color: #0b0f19; /* Cor de fundo caso a imagem demore a carregar */
  background-size: 100% 100%; /* Força o autoajuste perfeito à tela sem cortes */
  background-position: top center;
  background-repeat: no-repeat;
  transition: background-image 0.15s ease-in-out; /* Transição suave ao trocar de botão */
}
</style>