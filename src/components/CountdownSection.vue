<script setup lang="ts">
import { ref, computed, onMounted, onUnmounted } from 'vue'

// Target: Thursday June 18, 2026 at 11:30 AM IST (UTC+5:30)
const TARGET = new Date('2026-06-18T06:00:00Z') // 11:30 IST = 06:00 UTC

interface TimeLeft {
  days: number
  hours: number
  minutes: number
  seconds: number
}

const timeLeft = ref<TimeLeft>({ days: 0, hours: 0, minutes: 0, seconds: 0 })
const expired = ref(false)
let interval = 0

function calculate() {
  const diff = TARGET.getTime() - Date.now()
  if (diff <= 0) {
    expired.value = true
    timeLeft.value = { days: 0, hours: 0, minutes: 0, seconds: 0 }
    clearInterval(interval)
    return
  }
  const totalSec = Math.floor(diff / 1000)
  timeLeft.value = {
    days:    Math.floor(totalSec / 86400),
    hours:   Math.floor((totalSec % 86400) / 3600),
    minutes: Math.floor((totalSec % 3600) / 60),
    seconds: totalSec % 60,
  }
}

const pad = (n: number) => String(n).padStart(2, '0')

onMounted(() => {
  calculate()
  interval = window.setInterval(calculate, 1000)
})
onUnmounted(() => clearInterval(interval))
</script>

<template>
  <section class="section countdown">
    <div class="container">
      <p class="section-subtitle">counting down to</p>
      <h2 class="section-title">The Big Day</h2>
      <div class="divider"><span>♡</span></div>

      <div v-if="!expired" class="countdown__grid">
        <div class="countdown__block" v-for="(unit, label) in {
          Days:    timeLeft.days,
          Hours:   timeLeft.hours,
          Minutes: timeLeft.minutes,
          Seconds: timeLeft.seconds,
        }" :key="label">
          <div class="countdown__number">{{ pad(unit) }}</div>
          <div class="countdown__label">{{ label }}</div>
        </div>
      </div>

      <div v-else class="countdown__today">
        <p>Today is the day — Mabrook! 🌸</p>
      </div>

      <p class="countdown__date">Thursday · 18 June 2026 · 11:30 AM</p>
    </div>
  </section>
</template>

<style scoped>
.countdown {
  background: linear-gradient(135deg, #f0e8f5 0%, var(--color-cream) 100%);
  text-align: center;
}

.countdown__grid {
  display: flex;
  justify-content: center;
  gap: clamp(1rem, 4vw, 3rem);
  flex-wrap: wrap;
  margin-bottom: 2.5rem;
}

.countdown__block {
  display: flex;
  flex-direction: column;
  align-items: center;
  background: var(--color-white);
  border: 1px solid color-mix(in srgb, var(--color-mauve) 20%, transparent);
  border-radius: 1rem;
  padding: clamp(1rem, 3vw, 1.8rem) clamp(1.2rem, 4vw, 2.5rem);
  box-shadow:
    0 4px 24px -4px color-mix(in srgb, var(--color-mauve) 15%, transparent),
    inset 0 1px 0 rgba(255,255,255,0.8);
  min-width: 90px;
  position: relative;
  overflow: hidden;
}
.countdown__block::after {
  content: '';
  position: absolute;
  inset: 0;
  background: linear-gradient(135deg, rgba(197,160,40,0.04) 0%, transparent 60%);
  pointer-events: none;
}

.countdown__number {
  font-family: var(--font-serif);
  font-size: clamp(2.4rem, 6vw, 4.2rem);
  font-weight: 600;
  color: var(--color-plum);
  line-height: 1;
  letter-spacing: -0.02em;
  font-variant-numeric: tabular-nums;
}

.countdown__label {
  font-family: var(--font-serif);
  font-size: 0.72rem;
  letter-spacing: 0.22em;
  text-transform: uppercase;
  color: var(--color-mauve);
  margin-top: 0.5rem;
}

.countdown__today {
  font-family: var(--font-script);
  font-size: 2.5rem;
  color: var(--color-plum);
  margin-bottom: 2rem;
}

.countdown__date {
  font-family: var(--font-serif);
  font-size: 1rem;
  letter-spacing: 0.2em;
  text-transform: uppercase;
  color: var(--color-gold);
  margin-top: 0.5rem;
}
</style>
