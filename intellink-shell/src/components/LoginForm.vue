<template>
  <div id="loginView" class="login-container">
    <div class="brand-header">
      <h1>INTELLINK<span>.</span></h1>
      <p>Portal de Gestão Industrial & Licenciamento</p>
    </div>
    
    <form id="loginForm" @submit.prevent="handleLogin">
      <div class="input-group">
        <i class="fa-solid fa-building"></i>
        <input type="text" placeholder="Chave da Empresa / Licença" required value="BR-SOROCABA-JATOBA">
      </div>
      <div class="input-group">
        <i class="fa-solid fa-user"></i>
        <input type="text" id="username" placeholder="Usuário Master" required value="admin.master">
      </div>
      <div class="input-group">
        <i class="fa-solid fa-lock"></i>
        <input type="password" id="password" placeholder="Senha" required value="••••••••">
      </div>
      <div class="input-group">
        <i class="fa-solid fa-shield-halved"></i>
        <select required>
          <option value="email">Token via E-mail (Cadastrado)</option>
          <option value="sms">Token via SMS / Telefone</option>
        </select>
      </div>
      <button type="submit" class="btn-login">Aprovar & Acessar Dashboard</button>
    </form>
  </div>
</template>

<script setup>
  const handleLogin = () => {
    alert('Tentativa de login mockada.');
  };
</script>

<style scoped>
  /* AQUI ENTRA O CSS EXATO DO LOGIN TRANSPARENTE */
  .login-container {
    background: rgba(15,20,30,0.18);
    backdrop-filter: blur(16px);
    -webkit-backdrop-filter: blur(16px);
    border: 1px solid rgba(240, 246, 252, 0.1);
    padding: 2.5rem;
    border-radius: 12px;
    width: 100%;
    max-width: 420px;
    box-shadow: 0 20px 50px rgba(0,0,0,0.6);
    animation: fadeIn 0.5s ease-out;
  }

  .brand-header {
    text-align: center;
    margin-bottom: 2rem;
  }

  .brand-header h1 {
    font-size: 2rem;
    font-weight: 700;
    letter-spacing: 2px;
    color: #f0f6fc;
  }

  .brand-header h1 span { color: #0078ff; }
  .brand-header p {
    color: #8b949e;
    font-size: 0.85rem;
    margin-top: 0.5rem;
  }

  .input-group {
    margin-bottom: 1.25rem;
    position: relative;
  }

  .input-group i {
    position: absolute;
    left: 15px;
    top: 50%;
    transform: translateY(-50%);
    color: #8b949e;
  }

  .input-group input, .input-group select {
    width: 100%;
    padding: 14px 14px 14px 45px;
    background: rgba(0, 0, 0, 0.4);
    border: 1px solid rgba(240, 246, 252, 0.1);
    border-radius: 6px;
    color: #f0f6fc;
    font-size: 0.95rem;
    transition: all 0.3s;
  }

  .input-group input:focus, .input-group select:focus {
    outline: none;
    border-color: #0078ff;
    box-shadow: 0 0 0 3px rgba(0, 120, 255, 0.25);
  }

  .btn-login {
    width: 100%;
    padding: 14px;
    background: #0078ff;
    border: none;
    border-radius: 6px;
    color: white;
    font-weight: 600;
    font-size: 1rem;
    cursor: pointer;
    transition: background 0.3s;
  }

  .btn-login:hover { background: #0056b3; }

  @keyframes fadeIn {
    from { opacity: 0; transform: translateY(10px); }
    to { opacity: 1; transform: translateY(0); }
  }
</style>