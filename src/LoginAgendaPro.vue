<template>
  <div class="login-root">
    <div class="login-card">
      <h2 class="titulo">Entrar na plataforma</h2>

      <form @submit.prevent="submit">
        <label>E-mail</label>
        <input v-model="email" type="email" placeholder="seu@email.com.br" />

        <label>Senha</label>
        <div class="senha-box">
          <input :type="mostrar ? 'text' : 'password'" v-model="senha" />
          <button type="button" @click="mostrar = !mostrar">
            {{ mostrar ? 'Ocultar' : 'Mostrar' }}
          </button>
        </div>

        <button class="btn" type="submit">
          {{ loading ? 'Entrando...' : 'Entrar' }}
        </button>
      </form>

      <slot></slot>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'

const email = ref('')
const senha = ref('')
const mostrar = ref(false)
const loading = ref(false)

const emit = defineEmits(['login', 'navigate'])

async function submit() {
  if (!email.value || !senha.value) return

  loading.value = true

  emit('login', {
    email: email.value,
    senha: senha.value
  })

  loading.value = false
}
</script>

<style>
.login-root{
  min-height:100vh;
  display:flex;
  align-items:center;
  justify-content:center;
  background:#0f1115;
}
.login-card{
  width:380px;
  padding:32px;
  border-radius:12px;
  background:#1b1f27;
  color:white;
}
input{
  width:100%;
  height:42px;
  margin-bottom:12px;
  padding:8px;
  border-radius:6px;
  border:1px solid #333;
  background:#111;
  color:white;
}
.btn{
  width:100%;
  height:44px;
  background:#7a3cff;
  color:white;
  border:none;
  border-radius:8px;
}
</style>