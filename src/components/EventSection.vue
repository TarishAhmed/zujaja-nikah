<script setup lang="ts">
// Google Maps embed for Janakiram Auditorium, Chelakkara, Kerala
const MAP_SRC =
  'https://www.google.com/maps/embed?pb=!1m18!1m12!1m3!1d3920.5262741224255!2d76.33594327611895!3d10.693830960709647!2m3!1f0!2f0!3f0!3m2!1i1024!2i768!4f13.1!3m3!1m2!1s0x3ba7dd937e570c05%3A0xaccf35ac19f4a8d!2sJanakiram%20Auditorium!5e0!3m2!1sen!2sin!4v1778172748954!5m2!1sen!2sin'

const details = [
  { icon: '📅', label: 'Date', value: 'Thursday, 18 June 2026' },
  { icon: '🕐', label: 'Time', value: '11:30 AM onwards' },
  { icon: '📍', label: 'Venue', value: 'Janakiram Auditorium\nNear SMTHS School\nChelakkara, Kerala' },
]
</script>

<template>
  <section class="section event">
    <div class="container">
      <p class="section-subtitle">you are warmly invited</p>
      <h2 class="section-title">Nikah Details</h2>
      <div class="divider"><span>✦</span></div>

      <div class="event__cards">
        <div v-for="d in details" :key="d.label" class="event__card reveal">
          <span class="event__icon">{{ d.icon }}</span>
          <p class="event__card-label">{{ d.label }}</p>
          <p class="event__card-value">{{ d.value }}</p>
        </div>
      </div>

      <div class="event__map reveal">
        <iframe
          :src="MAP_SRC"
          class="event__iframe"
          title="Janakiram Auditorium, Chelakkara, Kerala"
          loading="lazy"
          referrerpolicy="no-referrer-when-downgrade"
          allowfullscreen
        />
      </div>
    </div>
  </section>
</template>

<style scoped>
.event {
  background: var(--color-cream);
  text-align: center;
}

.event__cards {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
  gap: 1.5rem;
  margin-bottom: 3rem;
}

.event__card {
  background: var(--color-white);
  border: 1px solid color-mix(in srgb, var(--color-mauve) 18%, transparent);
  border-radius: 1.2rem;
  padding: 2rem 1.5rem;
  box-shadow: 0 6px 30px -6px color-mix(in srgb, var(--color-mauve) 12%, transparent);
  position: relative;
  overflow: hidden;
  transition: transform 0.3s ease, box-shadow 0.3s ease;
}
.event__card:hover {
  transform: translateY(-4px);
  box-shadow: 0 12px 40px -8px color-mix(in srgb, var(--color-mauve) 22%, transparent);
}
.event__card::before {
  content: '';
  position: absolute;
  top: 0; left: 0; right: 0;
  height: 3px;
  background: linear-gradient(90deg, var(--color-mauve), var(--color-gold), var(--color-mauve));
}

.event__icon {
  font-size: 2rem;
  display: block;
  margin-bottom: 0.8rem;
}

.event__card-label {
  font-family: var(--font-serif);
  font-size: 0.72rem;
  letter-spacing: 0.22em;
  text-transform: uppercase;
  color: var(--color-mauve);
  margin-bottom: 0.5rem;
}

.event__card-value {
  font-family: var(--font-serif);
  font-size: clamp(0.95rem, 1.8vw, 1.1rem);
  font-weight: 600;
  color: var(--color-plum);
  white-space: pre-line;
  line-height: 1.5;
}

.event__map {
  border-radius: 1.2rem;
  overflow: hidden;
  box-shadow: 0 8px 40px -8px color-mix(in srgb, var(--color-plum) 20%, transparent);
  border: 1px solid color-mix(in srgb, var(--color-mauve) 18%, transparent);
}

.event__iframe {
  width: 100%;
  height: 420px;
  border: none;
  display: block;
}
</style>
