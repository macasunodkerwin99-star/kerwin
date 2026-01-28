<template>
  <div class="card">
    <h2>Register</h2>
    <form @submit.prevent="submit">
      <label>
        Name
        <input v-model="name" required />
      </label>
      <label>
        Email
        <input v-model="email" type="email" required />
      </label>
      <button type="submit">Register</button>
    </form>
  </div>
</template>

<script>
import { ref } from 'vue'
export default {
  emits: ['registered'],
  setup(_, { emit }) {
    const name = ref('')
    const email = ref('')

    function submit() {
      const user = { name: name.value, email: email.value }
      localStorage.setItem('gold_user', JSON.stringify(user))
      emit('registered')
    }

    return { name, email, submit }
  }
}
</script>

<style scoped>
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
}
</style>
