<script>
  import { onMount } from 'svelte'
  import { gsap } from 'gsap'
  import { ScrollTrigger } from 'gsap/ScrollTrigger'
  
  gsap.registerPlugin(ScrollTrigger)
  
  let heroRef
  let titleRef
  let subtitleRef
  let ctaRef
  let orbRef
  
  onMount(() => {
    const triggers = []
    
    // Initial entrance animation
    const tl = gsap.timeline({ delay: 0.3 })
    
    tl.fromTo('.hero-eyebrow', 
      { opacity: 0, y: 30 },
      { opacity: 1, y: 0, duration: 0.8, ease: 'expo.out' }
    )
    .fromTo('.title-word',
      { opacity: 0, y: 100, rotateX: -40 },
      { opacity: 1, y: 0, rotateX: 0, duration: 1, stagger: 0.1, ease: 'expo.out' },
      '-=0.4'
    )
    .fromTo('.hero-description',
      { opacity: 0, y: 30 },
      { opacity: 1, y: 0, duration: 0.8, ease: 'expo.out' },
      '-=0.6'
    )
    .fromTo('.hero-actions',
      { opacity: 0, y: 30 },
      { opacity: 1, y: 0, duration: 0.8, ease: 'expo.out' },
      '-=0.4'
    )
    .fromTo('.scroll-hint',
      { opacity: 0 },
      { opacity: 1, duration: 0.8 },
      '-=0.2'
    )
    
    // Parallax on scroll
    const st = ScrollTrigger.create({
      trigger: heroRef,
      start: 'top top',
      end: 'bottom top',
      scrub: true,
      onUpdate: (self) => {
        const progress = self.progress
        gsap.set(titleRef, { y: progress * 150 })
        gsap.set(subtitleRef, { y: progress * 100 })
        gsap.set(ctaRef, { y: progress * 50 })
        gsap.set(orbRef, { 
          y: progress * 200,
          scale: 1 + progress * 0.5,
          opacity: 1 - progress
        })
      }
    })
    
    triggers.push(st)
    
    // Mouse parallax for orb
    const handleMouseMove = (e) => {
      const x = (e.clientX / window.innerWidth - 0.5) * 30
      const y = (e.clientY / window.innerHeight - 0.5) * 30
      gsap.to(orbRef, {
        x,
        y,
        duration: 1,
        ease: 'power2.out'
      })
    }
    
    window.addEventListener('mousemove', handleMouseMove, { passive: true })
    
    return () => {
      triggers.forEach(t => t.kill())
      window.removeEventListener('mousemove', handleMouseMove)
    }
  })
</script>

<section bind:this={heroRef} id="hero" class="hero">
  <!-- Background gradient orb -->
  <div bind:this={orbRef} class="gradient-orb"></div>
  
  <!-- Grid pattern -->
  <div class="grid-pattern"></div>
  
  <div class="hero-content">
    <div class="hero-eyebrow">
      <span class="eyebrow-line"></span>
      <span class="eyebrow-text">Desarrollador Full Stack</span>
    </div>
    
    <h1 bind:this={titleRef} class="hero-title">
      <span class="title-word">Creando</span>
      <span class="title-word title-word-accent">experiencias</span>
      <span class="title-word">digitales</span>
    </h1>
    
    <p bind:this={subtitleRef} class="hero-description">
      Transformo ideas en productos digitales excepcionales. 
      Especializado en crear interfaces que no solo funcionan, 
      sino que se sienten intuitivas y memorables.
    </p>
    
    <div bind:this={ctaRef} class="hero-actions">
      <a href="#projects" class="btn btn-primary" data-cursor-hover>
        <span class="btn-text">Ver proyectos</span>
        <span class="btn-arrow">→</span>
      </a>
      <a href="#contact" class="btn btn-secondary" data-cursor-hover>
        Contactar
      </a>
    </div>
  </div>
  
  <!-- Scroll indicator -->
  <div class="scroll-hint">
    <div class="scroll-mouse">
      <div class="scroll-wheel"></div>
    </div>
    <span class="scroll-text">Scroll</span>
  </div>
  
  <!-- Corner decorations -->
  <div class="corner corner-tl"></div>
  <div class="corner corner-tr"></div>
  <div class="corner corner-bl"></div>
  <div class="corner corner-br"></div>
</section>

<style>
  .hero {
    position: relative;
    min-height: 100vh;
    display: flex;
    align-items: center;
    justify-content: center;
    padding: 2rem;
    overflow: hidden;
  }

  /* Gradient orb background */
  .gradient-orb {
    position: absolute;
    top: 20%;
    right: 10%;
    width: 700px;
    height: 700px;
    background: radial-gradient(
      circle at 30% 30%,
      rgba(255, 255, 255, 0.08) 0%,
      rgba(255, 255, 255, 0.02) 40%,
      transparent 70%
    );
    border-radius: 50%;
    pointer-events: none;
    will-change: transform, opacity;
    filter: blur(40px);
  }

  /* Grid pattern */
  .grid-pattern {
    position: absolute;
    inset: 0;
    background-image: 
      linear-gradient(rgba(255, 255, 255, 0.015) 1px, transparent 1px),
      linear-gradient(90deg, rgba(255, 255, 255, 0.015) 1px, transparent 1px);
    background-size: 80px 80px;
    mask-image: radial-gradient(ellipse at center, black 0%, transparent 70%);
    pointer-events: none;
  }

  .hero-content {
    position: relative;
    z-index: 1;
    max-width: 1000px;
    text-align: center;
    perspective: 1000px;
  }

  /* Eyebrow */
  .hero-eyebrow {
    display: inline-flex;
    align-items: center;
    gap: 1rem;
    margin-bottom: 2rem;
  }

  .eyebrow-line {
    width: 60px;
    height: 1px;
    background: linear-gradient(90deg, transparent, var(--accent-dim));
  }

  .eyebrow-text {
    font-family: var(--font-mono);
    font-size: 0.8125rem;
    letter-spacing: 0.3em;
    text-transform: uppercase;
    color: var(--text-secondary);
  }

  /* Title */
  .hero-title {
    font-size: clamp(3rem, 12vw, 8rem);
    font-weight: 700;
    line-height: 0.95;
    letter-spacing: -0.04em;
    margin-bottom: 2rem;
  }

  .title-word {
    display: block;
    transform-origin: center bottom;
    will-change: transform, opacity;
  }

  .title-word-accent {
    background: linear-gradient(135deg, #fff 0%, rgba(255,255,255,0.5) 100%);
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
    background-clip: text;
  }

  /* Description */
  .hero-description {
    font-size: 1.25rem;
    font-weight: 300;
    color: var(--text-secondary);
    max-width: 550px;
    margin: 0 auto 3rem;
    line-height: 1.8;
    will-change: transform, opacity;
  }

  /* Actions */
  .hero-actions {
    display: flex;
    gap: 1rem;
    justify-content: center;
    flex-wrap: wrap;
    will-change: transform, opacity;
  }

  .btn {
    display: inline-flex;
    align-items: center;
    gap: 0.75rem;
    padding: 1.1rem 2.2rem;
    font-size: 0.9375rem;
    font-weight: 500;
    text-decoration: none;
    border-radius: 100px;
    transition: all 0.4s var(--ease-expo-out);
    position: relative;
    overflow: hidden;
  }

  .btn::before {
    content: '';
    position: absolute;
    inset: 0;
    background: linear-gradient(135deg, transparent, rgba(255,255,255,0.1), transparent);
    transform: translateX(-100%);
    transition: transform 0.6s ease;
  }

  .btn:hover::before {
    transform: translateX(100%);
  }

  .btn-primary {
    background-color: var(--accent);
    color: var(--bg-primary);
  }

  .btn-primary:hover {
    transform: translateY(-3px);
    box-shadow: 0 20px 50px rgba(255, 255, 255, 0.15);
  }

  .btn-primary:active {
    transform: scale(0.97);
  }

  .btn-arrow {
    transition: transform 0.3s var(--ease-expo-out);
  }

  .btn:hover .btn-arrow {
    transform: translateX(6px);
  }

  .btn-secondary {
    background-color: transparent;
    color: var(--text-primary);
    border: 1px solid var(--border);
  }

  .btn-secondary:hover {
    border-color: var(--text-secondary);
    background-color: rgba(255, 255, 255, 0.03);
    transform: translateY(-3px);
  }

  /* Scroll hint */
  .scroll-hint {
    position: absolute;
    bottom: 3rem;
    left: 50%;
    transform: translateX(-50%);
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 0.75rem;
  }

  .scroll-mouse {
    width: 26px;
    height: 42px;
    border: 1.5px solid var(--text-muted);
    border-radius: 13px;
    display: flex;
    justify-content: center;
    padding-top: 10px;
  }

  .scroll-wheel {
    width: 4px;
    height: 8px;
    background-color: var(--text-secondary);
    border-radius: 2px;
    animation: scrollWheel 2s ease-in-out infinite;
  }

  @keyframes scrollWheel {
    0%, 100% { 
      transform: translateY(0);
      opacity: 1;
    }
    50% { 
      transform: translateY(10px);
      opacity: 0.3;
    }
  }

  .scroll-text {
    font-family: var(--font-mono);
    font-size: 0.6875rem;
    letter-spacing: 0.3em;
    text-transform: uppercase;
    color: var(--text-muted);
  }

  /* Corner decorations */
  .corner {
    position: absolute;
    width: 60px;
    height: 60px;
    border: 1px solid var(--border);
    opacity: 0.5;
  }

  .corner-tl {
    top: 2rem;
    left: 2rem;
    border-right: none;
    border-bottom: none;
  }

  .corner-tr {
    top: 2rem;
    right: 2rem;
    border-left: none;
    border-bottom: none;
  }

  .corner-bl {
    bottom: 2rem;
    left: 2rem;
    border-right: none;
    border-top: none;
  }

  .corner-br {
    bottom: 2rem;
    right: 2rem;
    border-left: none;
    border-top: none;
  }

  /* Responsive */
  @media (max-width: 768px) {
    .hero {
      padding: 6rem 1.5rem 4rem;
    }

    .gradient-orb {
      width: 400px;
      height: 400px;
      top: 10%;
      right: -20%;
    }

    .corner {
      display: none;
    }

    .hero-actions {
      flex-direction: column;
      width: 100%;
    }

    .btn {
      width: 100%;
      justify-content: center;
    }
  }
</style>
