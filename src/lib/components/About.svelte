<script>
  import { onMount } from 'svelte'
  import { gsap } from 'gsap'
  import { ScrollTrigger } from 'gsap/ScrollTrigger'
  
  gsap.registerPlugin(ScrollTrigger)
  
  let sectionRef
  let containerRef
  
  const slides = [
    {
      number: '01',
      title: 'Francisco González',
      subtitle: 'Desarrollador Full Stack',
      description: 'Especializado en crear soluciones digitales escalables con enfoque en performance y experiencia de usuario.'
    },
    {
      number: '02',
      title: '1+ Año',
      subtitle: 'De experiencia',
      description: 'Construyendo productos digitales para startups y empresas. Desde MVPs hasta plataformas empresariales complejas.'
    },
    {
      number: '03',
      title: '14+ Proyectos',
      subtitle: 'Entregados',
      description: 'Cada proyecto es una oportunidad para crear algo que trascienda. Código limpio, arquitectura sólida, resultados medibles.'
    },
    {
      number: '04',
      title: '10+ Clientes',
      subtitle: 'Satisfechos',
      description: 'Relaciones a largo plazo basadas en confianza, comunicación transparente y entrega constante de valor.'
    }
  ]
  
  onMount(() => {
    const ctx = gsap.context(() => {
      const cards = containerRef.querySelectorAll('.slide-card')
      
      const tl = gsap.timeline({
        scrollTrigger: {
          trigger: sectionRef,
          start: 'top top',
          end: `+=${slides.length * 100}%`,
          scrub: 1,
          pin: true,
          anticipatePin: 1,
        }
      })
      
      cards.forEach((card, i) => {
        if (i === 0) {
          tl.fromTo(card, 
            { opacity: 0, y: 100, scale: 0.9 },
            { opacity: 1, y: 0, scale: 1, duration: 0.5 }
          )
        } else {
          tl.to(cards[i - 1], {
            opacity: 0,
            y: -100,
            scale: 0.9,
            duration: 0.5
          })
          .fromTo(card,
            { opacity: 0, y: 100, scale: 0.9 },
            { opacity: 1, y: 0, scale: 1, duration: 0.5 },
            '<'
          )
        }
      })
    }, sectionRef)
    
    return () => ctx.revert()
  })
</script>

<section bind:this={sectionRef} id="about" class="about">
  <div class="about-header">
    <span class="section-tag">Sobre mí</span>
    <div class="progress-dots">
      {#each slides as _, i}
        <div class="dot"></div>
      {/each}
    </div>
  </div>
  
  <div bind:this={containerRef} class="slides-container">
    {#each slides as slide, i}
      <div class="slide-card">
        <span class="slide-number">{slide.number}</span>
        <h2 class="slide-title">{slide.title}</h2>
        <p class="slide-subtitle">{slide.subtitle}</p>
        <p class="slide-desc">{slide.description}</p>
      </div>
    {/each}
  </div>
  
  <div class="scroll-indicator">
    <span class="scroll-line"></span>
    <span class="scroll-text">Scroll</span>
  </div>
</section>

<style>
  .about {
    position: relative;
    height: 100vh;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    overflow: hidden;
    background: var(--bg-primary);
  }

  .about-header {
    position: absolute;
    top: 3rem;
    left: 50%;
    transform: translateX(-50%);
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 2rem;
    z-index: 10;
  }

  .section-tag {
    font-size: 0.75rem;
    letter-spacing: 0.3em;
    text-transform: uppercase;
    color: var(--text-muted);
  }

  .progress-dots {
    display: flex;
    gap: 0.75rem;
  }

  .dot {
    width: 8px;
    height: 8px;
    border-radius: 50%;
    background: var(--border);
  }

  .slides-container {
    position: relative;
    width: 100%;
    max-width: 800px;
    height: 400px;
    display: flex;
    align-items: center;
    justify-content: center;
  }

  .slide-card {
    position: absolute;
    text-align: center;
    padding: 2rem;
    will-change: transform, opacity;
  }

  .slide-number {
    display: block;
    font-size: 0.875rem;
    letter-spacing: 0.2em;
    color: var(--text-muted);
    margin-bottom: 2rem;
  }

  .slide-title {
    font-size: clamp(2.5rem, 6vw, 4.5rem);
    font-weight: 500;
    letter-spacing: -0.02em;
    margin-bottom: 1rem;
    background: linear-gradient(135deg, #fff 0%, rgba(255,255,255,0.8) 100%);
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
    background-clip: text;
  }

  .slide-subtitle {
    font-size: 1.25rem;
    color: var(--text-muted);
    margin-bottom: 1.5rem;
  }

  .slide-desc {
    font-size: 1.125rem;
    line-height: 1.7;
    color: var(--text-secondary);
    max-width: 500px;
    margin: 0 auto;
  }

  .scroll-indicator {
    position: absolute;
    bottom: 3rem;
    left: 50%;
    transform: translateX(-50%);
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 0.75rem;
  }

  .scroll-line {
    width: 1px;
    height: 40px;
    background: linear-gradient(to bottom, var(--text-muted), transparent);
    animation: pulse-line 2s ease-in-out infinite;
  }

  @keyframes pulse-line {
    0%, 100% { opacity: 0.5; transform: scaleY(1); }
    50% { opacity: 1; transform: scaleY(0.6); }
  }

  .scroll-text {
    font-size: 0.6875rem;
    letter-spacing: 0.2em;
    text-transform: uppercase;
    color: var(--text-muted);
  }

  @media (max-width: 768px) {
    .slide-title {
      font-size: 2.5rem;
    }

    .slide-desc {
      font-size: 1rem;
    }
  }
</style>
