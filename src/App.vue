<template>
  <div class="container">
    <h1>Gold Price Calculator</h1>
    <Auth v-if="!authenticated" @authenticated="onAuthenticated" />
    <Calculator v-else @logout="onLogout" />
  </div>
</template>

<script>
import { ref, onMounted } from 'vue'
import Auth from './components/Auth.vue'
import Calculator from './components/Calculator.vue'

export default {
  components: { Auth, Calculator },
  setup() {
    const authenticated = ref(false)

    onMounted(() => {
      authenticated.value = !!localStorage.getItem('gold_user')
    })

    function onAuthenticated() {
      authenticated.value = true
    }

    function onLogout() {
      localStorage.removeItem('gold_user')
      authenticated.value = false
    }

    return { authenticated, onAuthenticated, onLogout }
  }
}
</script>

<style>
.container {
  max-width: 520px;
  margin: 60px auto;
  padding: 48px;
  background: white;
  border-radius: 12px;
  box-shadow: 0 20px 60px rgba(15, 23, 42, 0.2), 0 0 1px rgba(15, 23, 42, 0.3);
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, 'Helvetica Neue', Arial, sans-serif;
  backdrop-filter: blur(10px);
}

h1 {
  margin: 0 0 36px 0;
  text-align: center;
  background: linear-gradient(135deg, #0ea5e9 0%, #0284c7 100%);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
  font-size: 32px;
  font-weight: 700;
  letter-spacing: -0.5px;
}
</style>
