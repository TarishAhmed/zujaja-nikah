<script setup lang="ts">
import { onMounted, onUnmounted } from 'vue'
import { gsap } from 'gsap'
import { ScrollTrigger } from 'gsap/ScrollTrigger'

import HeroSection     from '@/components/HeroSection.vue'
import CountdownSection from '@/components/CountdownSection.vue'
import EventSection    from '@/components/EventSection.vue'
import RingsScene      from '@/components/RingsScene.vue'
import RsvpSection     from '@/components/RsvpSection.vue'

gsap.registerPlugin(ScrollTrigger)

onMounted(() => {
  // Batch all .reveal elements — fade up as they enter viewport
  ScrollTrigger.batch('.reveal', {
    start:    'top 88%',
    onEnter: (els) =>
      gsap.to(els, {
        opacity:   1,
        y:         0,
        duration:  0.9,
        ease:      'power3.out',
        stagger:   0.12,
      }),
    once: true,
  })

  // Parallax on section backgrounds while scrolling
  gsap.utils.toArray<HTMLElement>('.section').forEach((section) => {
    gsap.to(section, {
      backgroundPositionY: '30%',
      ease: 'none',
      scrollTrigger: {
        trigger:  section,
        start:    'top bottom',
        end:      'bottom top',
        scrub:    true,
      },
    })
  })
})

onUnmounted(() => {
  ScrollTrigger.killAll()
})
</script>

<template>
  <main>
    <HeroSection />
    <CountdownSection />
    <RingsScene />
    <EventSection />
    <RsvpSection />

    <footer class="site-footer">
      <p class="site-footer__arabic" lang="ar">بَارَكَ اللَّهُ لَكُمَا وَبَارَكَ عَلَيْكُمَا وَجَمَعَ بَيْنَكُمَا فِي خَيْرٍ</p>
      <p class="site-footer__trans">May Allah bless you both, and bring you together in goodness.</p>
      <p class="site-footer__names">Zujaja &amp; Tarish · 18 June 2026</p>
    </footer>
  </main>
</template>

<style scoped>
main {
  display: flex;
  flex-direction: column;
}

.site-footer {
  background: var(--color-plum);
  color: var(--color-cream);
  text-align: center;
  padding: 3.5rem 1.5rem 2.5rem;
}

.site-footer__arabic {
  font-family: var(--font-arabic);
  font-size: clamp(1.1rem, 2.5vw, 1.6rem);
  direction: rtl;
  line-height: 2;
  color: var(--color-gold-lt);
  margin-bottom: 0.5rem;
}

.site-footer__trans {
  font-family: var(--font-serif);
  font-style: italic;
  font-size: 0.92rem;
  color: color-mix(in srgb, var(--color-cream) 70%, transparent);
  letter-spacing: 0.04em;
  margin-bottom: 1.5rem;
}

.site-footer__names {
  font-family: var(--font-script);
  font-size: 1.8rem;
  color: var(--color-mauve-lt);
}
</style>
