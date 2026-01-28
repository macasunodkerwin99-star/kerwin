<template>
  <div class="auth-container">
    <div class="card" v-if="!showRegister">
      <h2>Login</h2>
      <form @submit.prevent="login">
        <label>
          Email
          <input v-model="email" type="email" required />
        </label>
        <label>
          Password
          <input v-model="password" type="password" required />
        </label>
        <button type="submit">Login</button>
        <p class="error" v-if="error">{{ error }}</p>
      </form>
      <p class="toggle-link">
        Don't have an account?
        <button class="link-btn" @click="toggleMode">Register here</button>
      </p>
    </div>

    <div class="card" v-else>
      <h2>Register</h2>
      <form @submit.prevent="register">
        <label>
          Name
          <input v-model="name" required />
        </label>
        <label>
          Email
          <input v-model="email" type="email" required />
        </label>
        <label>
          Password
          <input v-model="password" type="password" required />
        </label>
        <label>
          Confirm Password
          <input v-model="confirmPassword" type="password" required />
        </label>
        <button type="submit">Register</button>
        <p class="error" v-if="error">{{ error }}</p>
      </form>
      <p class="toggle-link">
        Already have an account?
        <button class="link-btn" @click="toggleMode">Login here</button>
      </p>
    </div>
  </div>
</template>

<script>
import { ref } from 'vue'

export default {
  emits: ['authenticated'],
  setup(_, { emit }) {
    const showRegister = ref(false)
    const name = ref('')
    const email = ref('')
    const password = ref('')
    const confirmPassword = ref('')
    const error = ref('')

    function login() {
      const users = JSON.parse(localStorage.getItem('gold_users') || '{}')
      
      if (!users[email.value]) {
        error.value = 'Email not found. Please register first.'
        return
      }

      const user = users[email.value]
      if (user.password !== password.value) {
        error.value = 'Incorrect password.'
        return
      }

      localStorage.setItem('gold_user', JSON.stringify(user))
      emit('authenticated')
      error.value = ''
      clearForm()
    }

    function register() {
      if (!name.value.trim() || !email.value.trim() || !password.value || !confirmPassword.value) {
        error.value = 'Please fill in all fields.'
        return
      }

      if (password.value !== confirmPassword.value) {
        error.value = 'Passwords do not match.'
        return
      }

      if (password.value.length < 6) {
        error.value = 'Password must be at least 6 characters long.'
        return
      }

      const users = JSON.parse(localStorage.getItem('gold_users') || '{}')
      
      if (users[email.value]) {
        error.value = 'Email already registered. Please login.'
        return
      }

      const user = { name: name.value, email: email.value, password: password.value }
      users[email.value] = user
      localStorage.setItem('gold_users', JSON.stringify(users))
      localStorage.setItem('gold_user', JSON.stringify(user))
      emit('authenticated')
      error.value = ''
      clearForm()
    }

    function toggleMode() {
      showRegister.value = !showRegister.value
      clearForm()
    }

    function clearForm() {
      name.value = ''
      email.value = ''
      password.value = ''
      confirmPassword.value = ''
      error.value = ''
    }

    return { showRegister, name, email, password, confirmPassword, error, login, register, toggleMode }
  }
}
</script>

<style scoped>
.auth-container {
  width: 100%;
}

.card {
  padding: 0;
}

h2 {
  margin: 0 0 32px 0;
  color: #1e293b;
  font-size: 24px;
  font-weight: 700;
  letter-spacing: -0.3px;
}

form {
  display: flex;
  flex-direction: column;
  gap: 24px;
}

label {
  display: flex;
  flex-direction: column;
  gap: 10px;
  margin: 0;
  font-weight: 600;
  font-size: 12px;
  color: #334155;
  text-transform: uppercase;
  letter-spacing: 0.4px;
}

input {
  width: 100%;
  padding: 11px 14px;
  box-sizing: border-box;
  border: 2px solid #e2e8f0;
  border-radius: 6px;
  background: #f8fafc;
  font-size: 14px;
  transition: all 0.3s ease;
  color: #1e293b;
}

input:focus {
  outline: none;
  border-color: #0ea5e9;
  box-shadow: 0 0 0 3px rgba(14, 165, 233, 0.1), 0 0 20px rgba(14, 165, 233, 0.15);
  background: #fff;
}

button {
  width: 100%;
  padding: 11px;
  margin-top: 4px;
  cursor: pointer;
  font-weight: 600;
  font-size: 14px;
  border: none;
  border-radius: 6px;
  background: linear-gradient(135deg, #0ea5e9 0%, #0284c7 100%);
  color: white;
  transition: all 0.3s ease;
  box-shadow: 0 4px 12px rgba(14, 165, 233, 0.3);
}

button:hover:not(.link-btn) {
  transform: translateY(-2px);
  box-shadow: 0 6px 16px rgba(14, 165, 233, 0.4);
}

button:active:not(.link-btn) {
  transform: translateY(0);
}

.toggle-link {
  margin-top: 20px;
  text-align: center;
  color: #64748b;
  font-size: 14px;
}

.link-btn {
  background: none;
  border: none;
  color: #0ea5e9;
  cursor: pointer;
  font-weight: 600;
  padding: 0;
  box-shadow: none;
  transition: color 0.2s ease;
  margin-left: 4px;
}

.link-btn:hover {
  color: #0284c7;
  text-decoration: underline;
}

.error {
  color: #dc2626;
  font-size: 13px;
  margin: 0;
  padding: 10px;
  background: #fee2e2;
  border-radius: 4px;
  border-left: 3px solid #dc2626;
}
</style>
