<script setup lang="ts">
// Replace FORM_SRC with your Google Form embed URL once you've created the form.
// Go to: Form → Send → Embed (</>) → copy the src attribute from the <iframe> tag.
const FORM_SRC = 'https://docs.google.com/forms/d/e/1FAIpQLSejKvuMJpAPt5GVV5VeGhXaPtyTvmb7y3M4o0TB4ncSyc6gmQ/viewform?embedded=true' // ← paste your Google Form embed src here

const hasForm = Boolean(FORM_SRC)
</script>

<template>
  <section class="section rsvp" id="rsvp">
    <div class="container">
      <p class="section-subtitle">please let us know</p>
      <h2 class="section-title">RSVP</h2>
      <div class="divider"><span>♡</span></div>

      <p class="rsvp__intro reveal">
        Kindly confirm your attendance by <strong>June 1, 2026</strong>.<br>
        We look forward to celebrating with you.
      </p>

      <div v-if="hasForm" class="rsvp__frame-wrap reveal">
        <iframe
          :src="FORM_SRC"
          class="rsvp__iframe"
          title="RSVP — Zujaja & Tarish Nikah"
          frameborder="0"
          marginheight="0"
          marginwidth="0"
          loading="lazy"
        >
          Loading form…
        </iframe>
      </div>

      <!-- Placeholder shown until the Google Form URL is added -->
      <div v-else class="rsvp__placeholder reveal">
        <span class="rsvp__placeholder-icon">📋</span>
        <p>The RSVP form will appear here.</p>
        <p class="rsvp__placeholder-hint">
          Create a Google Form and paste the embed <code>src</code> URL into
          <code>RsvpSection.vue → FORM_SRC</code>.
        </p>
      </div>
    </div>
  </section>
</template>

<style scoped>
.rsvp {
  background: linear-gradient(180deg, var(--color-cream) 0%, #f0e8f5 100%);
  text-align: center;
}

.rsvp__intro {
  font-family: var(--font-serif);
  font-size: clamp(1rem, 2vw, 1.18rem);
  color: var(--color-plum-lt);
  margin-bottom: 2.5rem;
  line-height: 1.9;
}

/* Live Google Form embed */
.rsvp__frame-wrap {
  border-radius: 1.2rem;
  overflow: hidden;
  box-shadow: 0 8px 40px -8px color-mix(in srgb, var(--color-mauve) 20%, transparent);
  border: 1px solid color-mix(in srgb, var(--color-mauve) 18%, transparent);
}

.rsvp__iframe {
  width: 100%;
  height: 720px;
  border: none;
  display: block;
  background: var(--color-white);
}

/* Placeholder card */
.rsvp__placeholder {
  background: var(--color-white);
  border: 2px dashed color-mix(in srgb, var(--color-mauve) 30%, transparent);
  border-radius: 1.2rem;
  padding: 3rem 2rem;
  color: var(--color-plum-lt);
  max-width: 480px;
  margin-inline: auto;
}

.rsvp__placeholder-icon {
  font-size: 2.8rem;
  display: block;
  margin-bottom: 1rem;
}

.rsvp__placeholder p {
  font-family: var(--font-serif);
  font-size: 1.05rem;
  margin-bottom: 0.75rem;
}

.rsvp__placeholder-hint {
  font-size: 0.82rem !important;
  color: var(--color-mauve);
  line-height: 1.7;
}

code {
  background: color-mix(in srgb, var(--color-mauve) 12%, transparent);
  border-radius: 4px;
  padding: 0.1em 0.35em;
  font-size: 0.88em;
  font-family: monospace;
}
</style>
