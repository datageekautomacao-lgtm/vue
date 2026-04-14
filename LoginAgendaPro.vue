<template>
  <div class="login-root">
    <div class="login-card">
      <h2 class="titulo">Entrar na plataforma</h2>
      
      <form @submit.prevent="handleSubmit">
        <label>E-mail</label>
        <input 
          v-model="email" 
          type="email" 
          placeholder="seu@email.com.br" 
        />
        
        <label>Senha</label>
        <div class="senha-box">
          <input 
            :type="mostrarSenha ? 'text' : 'password'" 
            v-model="senha" 
          />
          <button 
            type="button" 
            @click="mostrarSenha = !mostrarSenha"
          >
            {{ mostrarSenha ? 'Ocultar' : 'Mostrar' }}
          </button>
        </div>

        <button class="btn" type="submit" :disabled="loading">
          {{ loading ? 'Entrando...' : 'Entrar' }}
        </button>
      </form>

      <!-- Slot para conteúdo adicional do WeWeb -->
      <slot></slot>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'

// Reactive variables
const email = ref('')
const senha = ref('')
const mostrarSenha = ref(false)
const loading = ref(false)

// Emits que o WeWeb consegue usar
const emit = defineEmits(['login-success', 'login-error'])

// Função principal
const handleSubmit = async () => {
  if (!email.value || !senha.value) {
    emit('login-error', 'Preencha e-mail e senha')
    return
  }

  loading.value = true

  // Envia os dados para o WeWeb poder capturar
  emit('login-success', {
    email: email.value,
    password: senha.value   // use "password" (mais padrão)
  })

  // Simula um pequeno delay (remova depois se quiser)
  setTimeout(() => {
    loading.value = false
  }, 800)
}
</script>

<style scoped>
.login-root {
  min-height: 100vh;
  display: flex;
  align-items: center;
  justify-content: center;
  background: #0f1115;
}

.login-card {
  width: 380px;
  padding: 32px;
  border-radius: 12px;
  background: #1b1f27;
  color: white;
}

label {
  display: block;
  margin-bottom: 6px;
  color: #ccc;
  font-size: 14px;
}

input {
  width: 100%;
  height: 42px;
  margin-bottom: 16px;
  padding: 0 12px;
  border-radius: 6px;
  border: 1px solid #444;
  background: #111;
  color: white;
  font-size: 15px;
}

.senha-box {
  position: relative;
  margin-bottom: 16px;
}

.senha-box button {
  position: absolute;
  right: 10px;
  top: 50%;
  transform: translateY(-50%);
  background: none;
  border: none;
  color: #7a3cff;
  cursor: pointer;
  font-size: 13px;
}

.btn {
  width: 100%;
  height: 44px;
  background: #7a3cff;
  color: white;
  border: none;
  border-radius: 8px;
  font-weight: 600;
  margin-top: 8px;
  cursor: pointer;
}

.btn:disabled {
  opacity: 0.7;
  cursor: not-allowed;
}
</style>
