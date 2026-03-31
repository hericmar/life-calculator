<script setup lang="ts">
import { reactive } from 'vue'

const form = reactive({
  gender: 'muz',
  age: 40,
  retirementAge: 67,
  sleepHours: 7.5,
  foodHours: 1.5,
  hygieneHours: 1,
  commuteHoursWeekly: 3.5,
  choresHoursWeekly: 7,
  workHoursWeekly: 40,
  tvHours: 1,
  entertainmentHours: 1,
})
</script>

<template>
  <div class="wrapper">
    <form class="card" @submit.prevent>
      <h1>⏳ Kalkulačka života</h1>

      <div class="field">
        <label>Jste</label>
        <div class="radio-group">
          <label><input type="radio" v-model="form.gender" value="muz" /> Muž</label>
          <label><input type="radio" v-model="form.gender" value="zena" /> Žena</label>
        </div>
      </div>

      <div class="field">
        <label>Kolik je Vám let?</label>
        <input type="number" v-model="form.age" min="0" max="120" />
      </div>

      <div class="field">
        <label>V kolika letech půjdete do důchodu?</label>
        <input type="number" v-model="form.retirementAge" min="0" max="120" />
        <span class="hint">Přesněji zjistíte zde: <a href="http://www.duchody-duchodci.cz/duchodovy-vek.php" target="_blank">duchody-duchodci.cz</a></span>
      </div>

      <div class="section-title">Denní rutina</div>

      <div class="field">
        <label>Kolik hodin denně spíte?</label>
        <input type="number" v-model="form.sleepHours" min="0" max="24" step="0.5" />
      </div>

      <div class="field">
        <label>Kolik hodin denně strávíte jídlem či jeho přípravou?</label>
        <input type="number" v-model="form.foodHours" min="0" max="24" step="0.5" />
      </div>

      <div class="field">
        <label>Kolik hodin denně strávíte osobní hygienou a péčí o sebe?</label>
        <input type="number" v-model="form.hygieneHours" min="0" max="24" step="0.5" />
      </div>

      <div class="field">
        <label>Kolik hodin denně strávíte u televize?</label>
        <input type="number" v-model="form.tvHours" min="0" max="24" step="0.5" />
      </div>

      <div class="field">
        <label>Kolik hodin denně strávíte „zábavou" u PC či na mobilu?</label>
        <input type="number" v-model="form.entertainmentHours" min="0" max="24" step="0.5" />
      </div>

      <div class="section-title">Týdenní rutina</div>

      <div class="field">
        <label>Kolik hodin týdně strávíte dojížděním?</label>
        <input type="number" v-model="form.commuteHoursWeekly" min="0" step="0.5" />
      </div>

      <div class="field">
        <label>Kolik hodin týdně potřebujete na zařízení povinností a údržbu domácnosti?</label>
        <input type="number" v-model="form.choresHoursWeekly" min="0" step="0.5" />
      </div>

      <div class="field">
        <label>Kolik hodin týdně odpracujete?</label>
        <input type="number" v-model="form.workHoursWeekly" min="0" step="0.5" />
      </div>

      <button type="submit">Vypočítat</button>
    </form>
  </div>
</template>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;600;700&display=swap');

* { box-sizing: border-box; }

.wrapper {
  min-height: 100vh;
  display: flex;
  align-items: center;
  justify-content: center;
  background: linear-gradient(135deg, #1a1a2e 0%, #16213e 50%, #0f3460 100%);
  padding: 2rem;
  font-family: 'Inter', sans-serif;
}

.card {
  background: rgba(255, 255, 255, 0.05);
  backdrop-filter: blur(16px);
  border: 1px solid rgba(255, 255, 255, 0.1);
  border-radius: 1.5rem;
  padding: 2.5rem;
  width: 100%;
  max-width: 540px;
  box-shadow: 0 25px 50px rgba(0, 0, 0, 0.4);
}

h1 {
  text-align: center;
  font-size: 1.8rem;
  font-weight: 700;
  color: #e0e0ff;
  margin-bottom: 2rem;
}

.section-title {
  font-size: 0.75rem;
  font-weight: 600;
  letter-spacing: 0.12em;
  text-transform: uppercase;
  color: #7c83ff;
  margin: 1.8rem 0 0.8rem;
  border-bottom: 1px solid rgba(124, 131, 255, 0.3);
  padding-bottom: 0.4rem;
}

.field {
  display: flex;
  flex-direction: column;
  gap: 0.35rem;
  margin-bottom: 1rem;
}

.field label {
  font-size: 0.875rem;
  color: #b0b8d8;
}

.field input[type='number'] {
  background: rgba(255, 255, 255, 0.07);
  border: 1px solid rgba(255, 255, 255, 0.15);
  border-radius: 0.6rem;
  color: #fff;
  padding: 0.55rem 0.85rem;
  font-size: 0.95rem;
  outline: none;
  transition: border-color 0.2s;
  width: 100%;
}

.field input[type='number']:focus {
  border-color: #7c83ff;
  box-shadow: 0 0 0 3px rgba(124, 131, 255, 0.2);
}

.radio-group {
  display: flex;
  gap: 1.5rem;
}

.radio-group label {
  display: flex;
  align-items: center;
  gap: 0.4rem;
  color: #b0b8d8;
  font-size: 0.9rem;
  cursor: pointer;
}

.radio-group input[type='radio'] {
  accent-color: #7c83ff;
  width: 1rem;
  height: 1rem;
}

.hint {
  font-size: 0.75rem;
  color: #666e9a;
}

.hint a {
  color: #7c83ff;
  text-decoration: none;
}

.hint a:hover { text-decoration: underline; }

button {
  margin-top: 1.8rem;
  width: 100%;
  padding: 0.8rem;
  background: linear-gradient(90deg, #7c83ff, #a78bfa);
  border: none;
  border-radius: 0.75rem;
  color: #fff;
  font-size: 1rem;
  font-weight: 600;
  cursor: pointer;
  transition: opacity 0.2s, transform 0.1s;
}

button:hover { opacity: 0.9; transform: translateY(-1px); }
button:active { transform: translateY(0); }
</style>
