<script>
  import { onMount } from 'svelte'
  import { gsap } from 'gsap'
  import { ScrollTrigger } from 'gsap/ScrollTrigger'
  import TextReveal from './TextReveal.svelte'
  
  gsap.registerPlugin(ScrollTrigger)
  
  let sectionRef
  
  const stats = [
    { number: '5+', label: 'Años de experiencia' },
    { number: '50+', label: 'Proyectos completados' },
    { number: '30+', label: 'Clientes satisfechos' },
    { number: '100%', label: 'Compromiso' }
  ]
  
  onMount(() => {
    const triggers = []
    
    // Section tag animation
    gsap.fromTo('.about-tag',
      { opacity: 0, x: -30 },
      {
        opacity: 1,
        x: 0,
        duration: 0.8,
        ease: 'expo.out',
        scrollTrigger: {
          trigger: sectionRef,
          start: 'top 80%',
          toggleActions: 'play none none reverse'
        }
      }
    )
    
    // Description paragraphs stagger
    gsap.fromTo('.about-text p',
      { opacity: 0, y: 40 },
      {
        opacity: 1,
        y: 0,
        duration: 0.8,
        stagger: 0.15,
        ease: 'expo.out',
        scrollTrigger: {
          trigger: '.about-text',
          start: 'top 80%',
          toggleActions: 'play none none reverse'
        }
      }
    )
    
    // Stats cards animation
    gsap.fromTo('.stat-card',
      { opacity: 0, y: 60, scale: 0.9 },
      {
        opacity: 1,
        y: 0,
        scale: 1,
        duration: 0.8,
        stagger: 0.1,
        ease: 'expo.out',
        scrollTrigger: {
          trigger: '.about-stats',
          start: 'top 80%',
          toggleActions: 'play none none reverse'
        }
      }
    )
    
    // Parallax effect on stats
    const st = ScrollTrigger.create({
      trigger: sectionRef,
      start: 'top bottom',
      end: 'bottom top',
      scrub: 1,
      onUpdate: (self) => {
        const cards = document.querySelectorAll('.stat-card')
        cards.forEach((card, i) => {
          const speed = 1 + i * 0.1
          const yPos = self.progress * 50 * speed
          gsap.set(card, { y: -yPos })
        })
      }
    })
    
    triggers.push(st)
    
    return () => {
      triggers.forEach(t => t.kill())
    }
  })
</script>

<section bind:this={sectionRef} id="about" class="about">
  <div class="about-content">
    <div class="about-grid">
      <div class="about-main">
        <span class="about-tag">Sobre mí</span>
        
        <h2 class="about-title">
          <TextReveal text="Pasión por crear" />
          <span class="title-secondary">
            <TextReveal text="productos digitales" className="text-dim" />
            <TextReveal text="excepcionales" />
          </span>
        </h2>
        
        <div class="about-text">
          <p>
            Soy un desarrollador full-stack con sede en [Tu Ciudad]. 
            Mi enfoque combina diseño minimalista con funcionalidad robusta, 
            creando experiencias que dejan una impresión duradera.
          </p>
          <p>
            Creo en el poder de los detalles. Cada línea de código, cada animación, 
            cada interacción está cuidadosamente considerada para crear algo 
            que no solo funcione perfectamente, sino que se sienta intuitivo.
          </p>
        </div>
        
        <a href="#contact" class="about-link" data-cursor-hover>
          <span>Trabajemos juntos</span>
          <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
            <path d="M7 17L17 7M17 7H7M17 7V17" stroke-linecap="round" stroke-linejoin="round"/>
          </svg>
        </a>
      </div>
      
      <div class="about-stats">
        {#each stats as stat, i}
          <div class="stat-card" style="--card-index: {i}">
            <div class="stat-glow"></div>
            <span class="stat-number">{stat.number}</span>
            <span class="stat-label">{stat.label}</span>
          </div>
        {/each}
      </div>
    </div>
  </div>
</section>

<style>
  .about {
    padding: var(--space-3xl) 2rem;
    position: relative;
    overflow: hidden;
  }

  .about-content {
    max-width: 1200px;
    margin: 0 auto;
  }

  .about-grid {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 6rem;
    align-items: start;
  }

  /* Main content */
  .about-main {
    display: flex;
    flex-direction: column;
    gap: 2rem;
    position: sticky;
    top: 30vh;
  }

  .about-tag {
    display: inline-flex;
    align-items: center;
    gap: 0.75rem;
    font-family: var(--font-mono);
    font-size: 0.8125rem;
    color: var(--text-muted);
    text-transform: uppercase;
    letter-spacing: 0.2em;
  }

  .about-tag::before {
    content: '';
    width: 40px;
    height: 1px;
    background-color: var(--accent);
  }

  .about-title {
    font-size: clamp(2.5rem, 5vw, 4rem);
    font-weight: 700;
    line-height: 1.1;
    letter-spacing: -0.03em;
  }

  .title-secondary {
    display: block;
    margin-top: 0.25rem;
  }

  .title-secondary :global(.text-dim) {
    color: var(--text-secondary) !important;
  }

  .about-text {
    display: flex;
    flex-direction: column;
    gap: 1.25rem;
  }

  .about-text p {
    font-size: 1.0625rem;
    color: var(--text-secondary);
    line-height: 1.8;
  }

  .about-link {
    display: inline-flex;
    align-items: center;
    gap: 0.75rem;
    margin-top: 1rem;
    font-size: 1rem;
    font-weight: 500;
    color: var(--accent);
    text-decoration: none;
    transition: gap 0.3s var(--ease-expo-out);
  }

  .about-link:hover {
    gap: 1rem;
  }

  .about-link svg {
    width: 20px;
    height: 20px;
    transition: transform 0.3s var(--ease-expo-out);
  }

  .about-link:hover svg {
    transform: translate(4px, -4px);
  }

  /* Stats section */
  .about-stats {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 1.5rem;
    padding-top: 4rem;
  }

  .stat-card {
    position: relative;
    padding: 2.5rem;
    background-color: var(--bg-tertiary);
    border: 1px solid var(--border);
    border-radius: 24px;
    display: flex;
    flex-direction: column;
    gap: 0.5rem;
    overflow: hidden;
    transition: all 0.5s var(--ease-expo-out);
    will-change: transform;
  }

  .stat-card:nth-child(2) {
    transform: translateY(40px);
  }

  .stat-card:nth-child(3) {
    transform: translateY(-20px);
  }

  .stat-card:nth-child(4) {
    transform: translateY(60px);
  }

  .stat-card:hover {
    border-color: var(--border-hover);
    transform: translateY(-8px) !important;
    box-shadow: 0 30px 60px rgba(0, 0, 0, 0.4);
  }

  .stat-glow {
    position: absolute;
    top: -50%;
    left: -50%;
    width: 200%;
    height: 200%;
    background: radial-gradient(
      circle at 50% 50%,
      rgba(255, 255, 255, 0.03) 0%,
      transparent 50%
    );
    opacity: 0;
    transition: opacity 0.5s ease;
  }

  .stat-card:hover .stat-glow {
    opacity: 1;
  }

  .stat-number {
    font-size: 3rem;
    font-weight: 700;
    background: linear-gradient(135deg, #fff 0%, rgba(255,255,255,0.5) 100%);
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
    background-clip: text;
    position: relative;
    z-index: 1;
  }

  .stat-label {
    font-size: 0.875rem;
    color: var(--text-muted);
    position: relative;
    z-index: 1;
  }

  /* Responsive */
  @media (max-width: 968px) {
    .about {
      padding: var(--space-2xl) 1.5rem;
    }

    .about-grid {
      grid-template-columns: 1fr;
      gap: 4rem;
    }

    .about-main {
      position: relative;
      top: 0;
    }

    .about-stats {
      padding-top: 0;
    }

    .stat-card:nth-child(2),
    .stat-card:nth-child(3),
    .stat-card:nth-child(4) {
      transform: none;
    }
  }

  @media (max-width: 480px) {
    .about-stats {
      grid-template-columns: 1fr;
    }
  }
</style>
