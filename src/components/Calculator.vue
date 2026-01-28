<template>
  <div class="card">
    <div class="header">
      <h2>Gold Calculator</h2>
      <button class="link" @click="$emit('logout')">Logout</button>
    </div>

    <div class="karats-container">
      <div v-for="karat in karats" :key="karat" class="karat-calculator">
        <div class="karat-header">
          <h3>{{ karat }} Karat</h3>
          <p class="rate-display">Rate: <strong>{{ karatRates[karat] }}₱/gram</strong></p>
        </div>

        <div class="form-group">
          <label>Grams</label>
          <input v-model.number="karatData[karat].grams" type="number" min="0" />
        </div>

        <div class="form-group">
          <label>Make (making charge)</label>
          <input v-model.number="karatData[karat].makeCharge" type="number" min="0" />
        </div>

        <button @click="calculate(karat)">Calculate</button>

        <div v-if="karatData[karat].calculated" class="result">
          <p>Subtotal (grams × rate + make): <strong>{{ karatData[karat].subtotalFormatted }}</strong></p>
          <p>Tax (12%): <strong>{{ karatData[karat].taxFormatted }}</strong></p>
          <p>Total: <strong>{{ karatData[karat].totalFormatted }}</strong></p>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { ref, computed } from 'vue'

export default {
  setup() {
    const karats = [12, 14, 18, 22, 24]
    
    const karatRates = {
      12: 4969.55,
      14: 5797.81,
      18: 7454.33,
      22: 9110.84,
      24: 9939.10
    }
    
    const karatData = ref({})
    
    // Initialize data for each karat
    karats.forEach(k => {
      karatData.value[k] = {
        grams: 0,
        makeCharge: 0,
        subtotal: 0,
        tax: 0,
        total: 0,
        calculated: false,
        subtotalFormatted: computed(() => '₱' + karatData.value[k].subtotal.toFixed(2)),
        taxFormatted: computed(() => '₱' + karatData.value[k].tax.toFixed(2)),
        totalFormatted: computed(() => '₱' + karatData.value[k].total.toFixed(2))
      }
    })

    function calculate(karat) {
      const data = karatData.value[karat]
      const s = (data.grams || 0) * karatRates[karat] + (data.makeCharge || 0)
      const t = s * 0.12
      data.subtotal = s
      data.tax = t
      data.total = s + t
      data.calculated = true
    }

    return { karats, karatRates, karatData, calculate }
  }
}
</script>

<style scoped>
.card {
  padding: 0;
}

.header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 36px;
  padding-bottom: 24px;
  border-bottom: 2px solid #e2e8f0;
}

.header h2 {
  margin: 0;
  color: #1e293b;
  font-size: 28px;
  font-weight: 700;
  letter-spacing: -0.5px;
}

.link {
  background: none;
  border: none;
  color: #0ea5e9;
  cursor: pointer;
  font-weight: 600;
  font-size: 12px;
  padding: 6px 10px;
  transition: all 0.3s ease;
  text-transform: uppercase;
  letter-spacing: 0.3px;
}

.link:hover {
  color: #0284c7;
  text-decoration: underline;
}

.karats-container {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(340px, 1fr));
  gap: 28px;
  margin-bottom: 20px;
}

.karat-calculator {
  background: linear-gradient(135deg, #f0f9ff 0%, #e0f2fe 100%);
  padding: 24px;
  border-radius: 10px;
  border: 2px solid #bae6fd;
  box-shadow: 0 4px 20px rgba(14, 165, 233, 0.08);
  transition: all 0.3s ease;
}

.karat-calculator:hover {
  box-shadow: 0 8px 30px rgba(14, 165, 233, 0.12);
  transform: translateY(-2px);
}

.karat-header {
  margin-bottom: 24px;
  padding-bottom: 16px;
  border-bottom: 2px solid #7dd3fc;
}

.karat-header h3 {
  margin: 0 0 8px 0;
  color: #0c4a6e;
  font-size: 20px;
  font-weight: 700;
  letter-spacing: -0.2px;
}

.rate-display {
  margin: 0;
  color: #0284c7;
  font-size: 13px;
  font-weight: 600;
  letter-spacing: 0.2px;
}

.rate-display strong {
  font-size: 14px;
  color: #0ea5e9;
}

.form-group {
  margin-bottom: 18px;
  display: flex;
  flex-direction: column;
}

.form-group label {
  margin-bottom: 8px;
  color: #0c4a6e;
  font-weight: 600;
  font-size: 12px;
  letter-spacing: 0.4px;
  text-transform: uppercase;
}

input {
  width: 100%;
  padding: 10px 12px;
  box-sizing: border-box;
  border: 2px solid #7dd3fc;
  border-radius: 6px;
  background: white;
  font-size: 14px;
  transition: all 0.3s ease;
  color: #0c4a6e;
}

input:focus {
  outline: none;
  border-color: #0284c7;
  box-shadow: 0 0 0 3px rgba(14, 165, 233, 0.2), 0 0 15px rgba(14, 165, 233, 0.15);
  background: #fff;
}

button {
  width: 100%;
  padding: 10px;
  margin-top: 8px;
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

button:hover {
  transform: translateY(-2px);
  box-shadow: 0 6px 16px rgba(14, 165, 233, 0.4);
}

button:active {
  transform: translateY(0);
}

.result {
  margin-top: 20px;
  background: white;
  padding: 20px;
  border-radius: 8px;
  border-left: 4px solid #0284c7;
  box-shadow: 0 2px 8px rgba(14, 165, 233, 0.1);
  animation: slideIn 0.4s ease;
}

.result p {
  margin: 12px 0;
  color: #0c4a6e;
  font-size: 13px;
  display: flex;
  justify-content: space-between;
  align-items: center;
  font-weight: 500;
}

.result strong {
  color: #0284c7;
  font-size: 16px;
  font-weight: 700;
  letter-spacing: -0.2px;
}

@keyframes slideIn {
  from {
    opacity: 0;
    transform: translateY(-12px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}
</style>
