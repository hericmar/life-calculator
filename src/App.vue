<script setup lang="ts">
import { reactive, computed } from 'vue'
import { Pie } from 'vue-chartjs'
import { Chart as ChartJS, ArcElement, Tooltip, Legend } from 'chart.js'

ChartJS.register(ArcElement, Tooltip, Legend)

const form = reactive({
  gender: 'muz',
  age: 40,
  retirementAge: 67,
  lifeExpectancy: 78,
  sleepHours: 7.5,
  foodHours: 1.5,
  hygieneHours: 1,
  commuteHoursWeekly: 3.5,
  choresHoursWeekly: 7,
  workHoursWeekly: 40,
  tvHours: 1,
  entertainmentHours: 1,
})

const chartData = computed(() => {
  const totalWeeklyHours = 168 // 24 * 7

  // Convert daily to weekly
  const sleepWeekly = form.sleepHours * 7
  const foodWeekly = form.foodHours * 7
  const hygieneWeekly = form.hygieneHours * 7
  const tvWeekly = form.tvHours * 7
  const entertainmentWeekly = form.entertainmentHours * 7

  const usedHours = sleepWeekly + foodWeekly + hygieneWeekly + tvWeekly + entertainmentWeekly +
    form.commuteHoursWeekly + form.choresHoursWeekly + form.workHoursWeekly

  const freeHours = Math.max(0, totalWeeklyHours - usedHours)

  return {
    labels: [
      `Sleep (${sleepWeekly.toFixed(1)}h)`,
      `Food (${foodWeekly.toFixed(1)}h)`,
      `Hygiene (${hygieneWeekly.toFixed(1)}h)`,
      `Work (${form.workHoursWeekly}h)`,
      `Commuting (${form.commuteHoursWeekly}h)`,
      `Household (${form.choresHoursWeekly}h)`,
      `TV (${tvWeekly.toFixed(1)}h)`,
      `PC/Phone (${entertainmentWeekly.toFixed(1)}h)`,
      `Free time (${freeHours.toFixed(1)}h)`,
    ],
    datasets: [{
      data: [
        sleepWeekly,
        foodWeekly,
        hygieneWeekly,
        form.workHoursWeekly,
        form.commuteHoursWeekly,
        form.choresHoursWeekly,
        tvWeekly,
        entertainmentWeekly,
        freeHours,
      ],
      backgroundColor: [
        '#6366f1', '#8b5cf6', '#a855f7', '#ec4899',
        '#f43f5e', '#f97316', '#eab308', '#84cc16', '#22c55e'
      ],
      borderWidth: 0,
    }]
  }
})

const chartOptions = {
  responsive: true,
  maintainAspectRatio: true,
  plugins: {
    legend: {
      position: 'bottom' as const,
      labels: {
        color: '#b0b8d8',
        padding: 12,
        font: { size: 11 }
      }
    }
  }
}

const freeTimePercent = computed(() => {
  const totalWeeklyHours = 168
  const sleepWeekly = form.sleepHours * 7
  const foodWeekly = form.foodHours * 7
  const hygieneWeekly = form.hygieneHours * 7
  const tvWeekly = form.tvHours * 7
  const entertainmentWeekly = form.entertainmentHours * 7
  const usedHours = sleepWeekly + foodWeekly + hygieneWeekly + tvWeekly + entertainmentWeekly +
    form.commuteHoursWeekly + form.choresHoursWeekly + form.workHoursWeekly
  const freeHours = Math.max(0, totalWeeklyHours - usedHours)
  return ((freeHours / totalWeeklyHours) * 100).toFixed(1)
})

const lifeChartData = computed(() => {
  const yearsLeft = Math.max(0, form.lifeExpectancy - form.age)
  const yearsToRetirement = Math.max(0, form.retirementAge - form.age)
  const yearsAfterRetirement = Math.max(0, yearsLeft - yearsToRetirement)
  const yearsLived = form.age

  const weeksPerYear = 52
  const hoursPerWeek = 168

  // Weekly hours breakdown
  const sleepWeekly = form.sleepHours * 7
  const foodWeekly = form.foodHours * 7
  const hygieneWeekly = form.hygieneHours * 7
  const tvWeekly = form.tvHours * 7
  const entertainmentWeekly = form.entertainmentHours * 7

  // Total hours in expected life
  const livedHours = yearsLived * weeksPerYear * hoursPerWeek

  // Calculate totals for working years
  const workingWeeks = yearsToRetirement * weeksPerYear
  const retiredWeeks = yearsAfterRetirement * weeksPerYear
  const totalWeeks = workingWeeks + retiredWeeks

  // Sleep, food, hygiene, tv, entertainment apply to all remaining life
  const totalSleep = sleepWeekly * totalWeeks
  const totalFood = foodWeekly * totalWeeks
  const totalHygiene = hygieneWeekly * totalWeeks
  const totalTv = tvWeekly * totalWeeks
  const totalEntertainment = entertainmentWeekly * totalWeeks

  // Work, commute, chores only during working years
  const totalWork = form.workHoursWeekly * workingWeeks
  const totalCommute = form.commuteHoursWeekly * workingWeeks
  const totalChores = form.choresHoursWeekly * totalWeeks

  const totalUsed = totalSleep + totalFood + totalHygiene + totalTv + totalEntertainment + totalWork + totalCommute + totalChores
  const totalFree = Math.max(0, (yearsLeft * weeksPerYear * hoursPerWeek) - totalUsed)

  const toYears = (hours: number) => (hours / (weeksPerYear * hoursPerWeek)).toFixed(1)

  return {
    labels: [
      `Already lived (${toYears(livedHours)} years)`,
      `Sleep (${toYears(totalSleep)} years)`,
      `Food (${toYears(totalFood)} years)`,
      `Hygiene (${toYears(totalHygiene)} years)`,
      `Work (${toYears(totalWork)} years)`,
      `Commuting (${toYears(totalCommute)} years)`,
      `Household (${toYears(totalChores)} years)`,
      `TV (${toYears(totalTv)} years)`,
      `PC/Phone (${toYears(totalEntertainment)} years)`,
      `Free time (${toYears(totalFree)} years)`,
    ],
    datasets: [{
      data: [
        livedHours, totalSleep, totalFood, totalHygiene, totalWork, totalCommute, totalChores, totalTv, totalEntertainment, totalFree
      ],
      backgroundColor: [
        '#475569', '#6366f1', '#8b5cf6', '#a855f7', '#ec4899',
        '#f43f5e', '#f97316', '#eab308', '#84cc16', '#22c55e'
      ],
      borderWidth: 0,
    }]
  }
})

const freeYearsLeft = computed(() => {
  const yearsLeft = Math.max(0, form.lifeExpectancy - form.age)
  const yearsToRetirement = Math.max(0, form.retirementAge - form.age)
  const yearsAfterRetirement = Math.max(0, yearsLeft - yearsToRetirement)
  const weeksPerYear = 52
  const hoursPerWeek = 168

  const sleepWeekly = form.sleepHours * 7
  const foodWeekly = form.foodHours * 7
  const hygieneWeekly = form.hygieneHours * 7
  const tvWeekly = form.tvHours * 7
  const entertainmentWeekly = form.entertainmentHours * 7

  const totalHoursLeft = yearsLeft * weeksPerYear * hoursPerWeek
  const workingWeeks = yearsToRetirement * weeksPerYear
  const totalWeeks = workingWeeks + yearsAfterRetirement * weeksPerYear

  const totalUsed = (sleepWeekly + foodWeekly + hygieneWeekly + tvWeekly + entertainmentWeekly) * totalWeeks +
    (form.workHoursWeekly + form.commuteHoursWeekly) * workingWeeks + form.choresHoursWeekly * totalWeeks

  const totalFree = Math.max(0, totalHoursLeft - totalUsed)
  return (totalFree / (weeksPerYear * hoursPerWeek)).toFixed(1)
})
</script>

<template>
  <div class="wrapper">
    <form class="card" @submit.prevent>
      <h1>⏳ Life Calculator</h1>

      <div class="field">
        <label>How old are you?</label>
        <input type="number" v-model="form.age" min="0" max="120" />
      </div>

      <div class="field">
        <label>At what age will you retire?</label>
        <input type="number" v-model="form.retirementAge" min="0" max="120" />
      </div>

      <div class="field">
        <label>Life expectancy</label>
        <input type="number" v-model="form.lifeExpectancy" min="0" max="120" />
        <span class="hint">Average: men ~76 years, women ~82 years</span>
      </div>

      <div class="section-title">Daily routine</div>

      <div class="field">
        <label>How many hours do you sleep per day?</label>
        <input type="number" v-model="form.sleepHours" min="0" max="24" step="0.5" />
      </div>

      <div class="field">
        <label>How many hours per day do you spend eating or preparing food?</label>
        <input type="number" v-model="form.foodHours" min="0" max="24" step="0.5" />
      </div>

      <div class="field">
        <label>How many hours per day do you spend on personal hygiene and self-care?</label>
        <input type="number" v-model="form.hygieneHours" min="0" max="24" step="0.5" />
      </div>

      <div class="field">
        <label>How many hours per day do you watch TV?</label>
        <input type="number" v-model="form.tvHours" min="0" max="24" step="0.5" />
      </div>

      <div class="field">
        <label>How many hours per day do you spend on “fun” on your PC or phone?</label>
        <input type="number" v-model="form.entertainmentHours" min="0" max="24" step="0.5" />
      </div>

      <div class="section-title">Weekly routine</div>

      <div class="field">
        <label>How many hours per week do you spend commuting?</label>
        <input type="number" v-model="form.commuteHoursWeekly" min="0" step="0.5" />
      </div>

      <div class="field">
        <label>How many hours per week do you need for errands and household maintenance?</label>
        <input type="number" v-model="form.choresHoursWeekly" min="0" step="0.5" />
      </div>

      <div class="field">
        <label>How many hours per week do you work?</label>
        <input type="number" v-model="form.workHoursWeekly" min="0" step="0.5" />
      </div>
    </form>

    <div class="charts-column">
      <div class="card chart-card">
        <h2>📊 Your week</h2>
        <Pie :data="chartData" :options="chartOptions" />
        <p class="summary">You have <strong>{{ freeTimePercent }}%</strong> free time per week</p>
      </div>

      <div class="card chart-card">
        <h2>🧬 Your life (overview)</h2>
        <Pie :data="lifeChartData" :options="chartOptions" />
        <p class="summary">You have <strong>{{ freeYearsLeft }} years</strong> of free time left</p>
      </div>
    </div>
  </div>
</template>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;600;700&display=swap');

* { box-sizing: border-box; }

.wrapper {
  min-height: 100vh;
  display: flex;
  align-items: flex-start;
  justify-content: center;
  gap: 2rem;
  flex-wrap: wrap;
  background: linear-gradient(135deg, #1a1a2e 0%, #16213e 50%, #0f3460 100%);
  padding: 2rem;
  font-family: 'Inter', sans-serif;
}

.charts-column {
  display: flex;
  flex-direction: column;
  gap: 2rem;
  width: 100%;
  max-width: 480px;
}

.card {
  background: rgba(255, 255, 255, 0.05);
  backdrop-filter: blur(16px);
  border: 1px solid rgba(255, 255, 255, 0.1);
  border-radius: 1.5rem;
  padding: 2.5rem;
  width: 100%;
  max-width: 480px;
  box-shadow: 0 25px 50px rgba(0, 0, 0, 0.4);
}

.chart-card {
  display: flex;
  flex-direction: column;
  align-items: center;
}

h1, h2 {
  text-align: center;
  font-weight: 700;
  color: #e0e0ff;
  margin-bottom: 1.5rem;
}

h1 { font-size: 1.8rem; }
h2 { font-size: 1.4rem; }

.summary {
  margin-top: 1.5rem;
  color: #b0b8d8;
  font-size: 1rem;
}

.summary strong {
  color: #22c55e;
  font-size: 1.2rem;
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
</style>
