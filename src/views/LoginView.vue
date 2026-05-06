<template>
  <div class="login-container">
    <form class="login-form" @submit.prevent="handleLogin">
      <h1>Entrar</h1>

      <div v-if="errorMessage" class="error-message">{{ errorMessage }}</div>

      <div class="field">
        <label for="email">Email</label>
        <input
          id="email"
          v-model="email"
          type="email"
          placeholder="seu@email.com"
          required
          autocomplete="email"
        />
      </div>

      <div class="field">
        <label for="password">Senha</label>
        <input
          id="password"
          v-model="password"
          type="password"
          placeholder="••••••••"
          required
          autocomplete="current-password"
        />
      </div>

      <button type="submit" :disabled="loading">
        {{ loading ? 'Entrando...' : 'Entrar' }}
      </button>
    </form>
  </div>
</template>

<script setup>
import { ref } from 'vue';
import { useRouter } from 'vue-router';
import { useAuthStore } from '@/stores/auth';

const router = useRouter();
const authStore = useAuthStore();

const email = ref('');
const password = ref('');
const loading = ref(false);
const errorMessage = ref('');

async function handleLogin() {
  loading.value = true;
  errorMessage.value = '';
  try {
    await authStore.login(email.value, password.value);
    router.push('/');
  } catch (err) {
    errorMessage.value =
      err.response?.data?.detail ??
      'Erro ao entrar. Verifique suas credenciais.';
  } finally {
    loading.value = false;
  }
}
</script>

<style scoped>
/* Reset básico */
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: 'Inter', 'Segoe UI', sans-serif;
}

/* Container centralizado */
.login-container {
  display: flex;
}

/* Card do formulário */
.login-form {
  padding: 2.5rem;
  border-radius: 12px;
  width: 100%;
  max-width: 380px;
}

/* Título */
.login-form h1 {
  margin-bottom: 1rem;
  color: #a51357;
  font-size: 1.3rem;
}

/* Campos */
.field {
  margin-bottom: 1.2rem;
  display: flex;
  flex-direction: column;
}

.field label {
  font-size: 0.9rem;
  margin-bottom: 0.4rem;
  color: #7b043a;
}

.field input {
  padding: 0.7rem 0.8rem;
  border-radius: 8px;
  border: 1px solid #cfabb8;
  font-size: 0.95rem;
  transition: all 0.2s ease;
}

/* Efeito foco */
.field input:focus {
  outline: none;
  border-color: #d53d7f;
  box-shadow: 0 0 0 2px rgba(99, 102, 241, 0.2);
}

/* Botão */
button {
  width: 100%;
  padding: 0.75rem;
  border: none;
  border-radius: 8px;
  background: #d53d7f;
  color: #fff;
  font-size: 1rem;
  font-weight: 500;
  cursor: pointer;
  transition: background 0.2s ease, transform 0.1s ease;
}

button:active:not(:disabled) {
  transform: scale(0.98);
}

/* Estado desabilitado */
button:disabled {
  background: #a5b4fc;
  cursor: not-allowed;
}

/* Mensagem de erro */
.error-message {
  background: #fee2e2;
  color: #b91c1c;
  padding: 0.6rem;
  border-radius: 6px;
  font-size: 0.85rem;
  margin-bottom: 1rem;
  text-align: center;
}
</style>
