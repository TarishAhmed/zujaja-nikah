<script setup lang="ts">
import { ref, onMounted, onUnmounted } from 'vue'
import { gsap } from 'gsap'

// ─── Petal canvas ────────────────────────────────────────────────
const canvasRef = ref<HTMLCanvasElement | null>(null)
let animFrame = 0

interface Petal {
  x: number
  y: number
  size: number
  speedY: number
  speedX: number
  opacity: number
  angle: number
  angleSpeed: number
  color: string
}

const PETAL_COLORS = ['#C4A4CA', '#9B7BA0', '#D4B8D8', '#E8D5EC', '#C5A028']

function makePetal(width: number): Petal {
  return {
    x: Math.random() * width,
    y: -20 - Math.random() * 120,
    size: 6 + Math.random() * 10,
    speedY: 0.8 + Math.random() * 1.4,
    speedX: -0.5 + Math.random() * 1.0,
    opacity: 0.4 + Math.random() * 0.5,
    angle: Math.random() * Math.PI * 2,
    angleSpeed: (Math.random() - 0.5) * 0.04,
    color: PETAL_COLORS[Math.floor(Math.random() * PETAL_COLORS.length)] ?? '#C4A4CA',
  }
}

function drawPetal(ctx: CanvasRenderingContext2D, p: Petal) {
  ctx.save()
  ctx.globalAlpha = p.opacity
  ctx.translate(p.x, p.y)
  ctx.rotate(p.angle)
  ctx.beginPath()
  ctx.ellipse(0, 0, p.size * 0.5, p.size, 0, 0, Math.PI * 2)
  ctx.fillStyle = p.color
  ctx.fill()
  ctx.restore()
}

onMounted(() => {
  const canvas = canvasRef.value!
  const ctx = canvas.getContext('2d')!
  let petals: Petal[] = []

  function resize() {
    canvas.width = canvas.offsetWidth
    canvas.height = canvas.offsetHeight
  }
  resize()
  window.addEventListener('resize', resize)

  const COUNT = 55
  for (let i = 0; i < COUNT; i++) {
    const p = makePetal(canvas.width)
    p.y = Math.random() * canvas.height // scatter on load
    petals.push(p)
  }

  function loop() {
    ctx.clearRect(0, 0, canvas.width, canvas.height)
    for (const p of petals) {
      p.y += p.speedY
      p.x += p.speedX
      p.angle += p.angleSpeed
      drawPetal(ctx, p)
      if (p.y > canvas.height + 20) {
        Object.assign(p, makePetal(canvas.width))
      }
    }
    animFrame = requestAnimationFrame(loop)
  }
  loop()

  // ─── Hero entrance animation ─────────────────────────────────
  const tl = gsap.timeline({ defaults: { ease: 'power3.out' } })
  tl.fromTo('.hero__bismillah', { opacity: 0, y: -30 }, { opacity: 1, y: 0, duration: 1.2 })
    .fromTo('.hero__invite', { opacity: 0 }, { opacity: 1, duration: 0.8 }, '-=0.4')
    .fromTo('.hero__parents', { opacity: 0 }, { opacity: 1, duration: 0.8 }, '-=0.4')
    .fromTo('.hero__bride', { opacity: 0, x: -60 }, { opacity: 1, x: 0, duration: 1 }, '-=0.3')
    .fromTo('.hero__amp', { opacity: 0, scale: 0.5 }, { opacity: 1, scale: 1, duration: 0.6 }, '-=0.6')
    .fromTo('.hero__groom', { opacity: 0, x: 60 }, { opacity: 1, x: 0, duration: 1 }, '-=0.7')
    .fromTo('.hero__scroll-hint', { opacity: 0, y: 10 }, { opacity: 1, y: 0, duration: 0.8 }, '+=0.4')

  // Parallax on mouse move
  const hero = document.querySelector('.hero') as HTMLElement
  const handleMove = (e: MouseEvent) => {
    const cx = e.clientX / window.innerWidth - 0.5
    const cy = e.clientY / window.innerHeight - 0.5
    gsap.to('.hero__layer--1', { x: cx * 18, y: cy * 12, duration: 1.2, ease: 'power2.out' })
    gsap.to('.hero__layer--2', { x: cx * -12, y: cy * -8, duration: 1.2, ease: 'power2.out' })
    gsap.to('.hero__layer--3', { x: cx * 28, y: cy * 18, duration: 1.4, ease: 'power2.out' })
  }
  hero.addEventListener('mousemove', handleMove)

  onUnmounted(() => {
    cancelAnimationFrame(animFrame)
    window.removeEventListener('resize', resize)
    hero.removeEventListener('mousemove', handleMove)
  })
})
</script>

<template>
  <section class="hero">
    <!-- Falling petals canvas -->
    <canvas ref="canvasRef" class="hero__canvas" aria-hidden="true" />

    <!-- Decorative parallax blobs -->
    <div class="hero__layer hero__layer--1" aria-hidden="true" />
    <div class="hero__layer hero__layer--2" aria-hidden="true" />
    <div class="hero__layer hero__layer--3" aria-hidden="true" />

    <!-- Content -->
    <div class="hero__content">
      <p class="hero__bismillah" lang="ar">بِسْمِ اللهِ الرَّحْمٰنِ الرَّحِيْم</p>

      <p class="hero__invite">
        In the name of Allah, the Most Beneficent, the Most Merciful.<br>
        With the blessings of Allah Almighty,
      </p>

      <p class="hero__parents">
        Mr. K Nasir Ahmad &amp; Mrs. Kadeeja PM<br>
        <span class="hero__parents-invite">cordially invite you and your family to grace<br>the auspicious occasion of the Nikkah Ceremony</span>
      </p>

      <div class="divider"><span>✦</span></div>

      <h1 class="hero__bride">Zujaja K</h1>
      <p class="hero__amp">&amp;</p>
      <h1 class="hero__groom">Tarish Ahmed B</h1>

      <p class="hero__groom-parents">Son of Mr. BB Ahmed Kabeer &amp; Mrs. TP Subaida</p>

      <p class="hero__scroll-hint" aria-hidden="true">scroll to explore ↓</p>
    </div>
  </section>
</template>

<style scoped>
/* ─── Hero wrapper ───────────────────────────────────────────── */
.hero {
  position: relative;
  min-height: 100svh;
  display: flex;
  align-items: center;
  justify-content: center;
  background: linear-gradient(160deg, #f9f4ee 0%, #ede3f0 60%, #f5f0e8 100%);
  overflow: hidden;
}

/* Petal canvas */
.hero__canvas {
  position: absolute;
  inset: 0;
  width: 100%;
  height: 100%;
  pointer-events: none;
  z-index: 1;
}

/* Parallax decorative blobs */
.hero__layer {
  position: absolute;
  border-radius: 50%;
  filter: blur(60px);
  opacity: 0.22;
  pointer-events: none;
}
.hero__layer--1 {
  width: 520px; height: 520px;
  background: radial-gradient(circle, #c4a4ca 0%, transparent 70%);
  top: -120px; left: -160px;
  z-index: 0;
}
.hero__layer--2 {
  width: 400px; height: 400px;
  background: radial-gradient(circle, #c5a028 0%, transparent 70%);
  bottom: -100px; right: -120px;
  z-index: 0;
}
.hero__layer--3 {
  width: 280px; height: 280px;
  background: radial-gradient(circle, #9b7ba0 0%, transparent 70%);
  top: 30%; left: 70%;
  z-index: 0;
}

/* Content */
.hero__content {
  position: relative;
  z-index: 2;
  text-align: center;
  padding: 2.5rem 1.5rem;
  max-width: 680px;
}

.hero__bismillah {
  font-family: var(--font-arabic);
  font-size: clamp(1.8rem, 4vw, 2.8rem);
  color: var(--color-plum);
  letter-spacing: 0.04em;
  margin-bottom: 1.2rem;
  direction: rtl;
}

.hero__invite {
  font-family: var(--font-serif);
  font-size: clamp(0.78rem, 1.5vw, 0.92rem);
  letter-spacing: 0.14em;
  text-transform: uppercase;
  color: var(--color-plum-lt);
  margin-bottom: 1.2rem;
  line-height: 1.8;
}

.hero__parents {
  font-family: var(--font-serif);
  font-size: clamp(1rem, 2.2vw, 1.25rem);
  font-weight: 600;
  color: var(--color-plum);
  margin-bottom: 0.4rem;
  line-height: 1.5;
}
.hero__parents-invite {
  font-weight: 300;
  font-style: italic;
  font-size: 0.88em;
  display: block;
  margin-top: 0.3em;
}

.hero__bride,
.hero__groom {
  font-family: var(--font-script);
  font-size: clamp(3.2rem, 8vw, 6.5rem);
  color: var(--color-plum);
  line-height: 1;
  font-weight: 400;
}

.hero__amp {
  font-family: var(--font-serif);
  font-size: clamp(1.6rem, 3.5vw, 2.8rem);
  color: var(--color-gold);
  margin-block: 0.15em;
  letter-spacing: 0.1em;
}

.hero__groom-parents {
  font-family: var(--font-serif);
  font-size: 1rem;
  font-style: italic;
  color: var(--color-mauve);
  opacity: 0.75;
  margin-top: 0.5rem;
  letter-spacing: 0.05em;
}

.hero__scroll-hint {
  margin-top: 2.5rem;
  font-family: var(--font-serif);
  font-size: 0.82rem;
  letter-spacing: 0.2em;
  text-transform: uppercase;
  color: var(--color-mauve);
  animation: bob 2s ease-in-out infinite;
}

@keyframes bob {
  0%, 100% { transform: translateY(0); }
  50%       { transform: translateY(6px); }
}
</style>
