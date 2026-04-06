<script>
  import { onMount } from 'svelte'
  import { gsap } from 'gsap'
  import { ScrollTrigger } from 'gsap/ScrollTrigger'
  
  gsap.registerPlugin(ScrollTrigger)
  
  const technologies = [
    { name: 'React', category: 'frontend', color: '#61DAFB' },
    { name: 'Svelte', category: 'frontend', color: '#FF3E00' },
    { name: 'JavaScript', category: 'frontend', color: '#F7DF1E' },
    { name: 'HTML', category: 'frontend', color: '#E34F26' },
    { name: 'CSS', category: 'frontend', color: '#1572B6' },
    { name: 'Tailwind', category: 'frontend', color: '#38B2AC' },
    { name: 'Bootstrap', category: 'frontend', color: '#7952B3' },
    { name: 'Node.js', category: 'backend', color: '#339933' },
    { name: 'PHP', category: 'backend', color: '#777BB4' },
    { name: 'MySQL', category: 'backend', color: '#4479A1' },
    { name: 'MongoDB', category: 'backend', color: '#47A248' },
    { name: 'PostgreSQL', category: 'backend', color: '#336791' },
    { name: 'Docker', category: 'tools', color: '#2496ED' },
    { name: 'Git', category: 'tools', color: '#F05032' },
    { name: 'AWS', category: 'tools', color: '#FF9900' }
  ]
  
  onMount(() => {
    gsap.fromTo('.skills-header',
      { opacity: 0, y: 50 },
      {
        opacity: 1,
        y: 0,
        duration: 0.8,
        ease: 'expo.out',
        scrollTrigger: {
          trigger: '.skills-header',
          start: 'top 85%'
        }
      }
    )
    
    gsap.fromTo('.tech-card',
      { opacity: 0, scale: 0.9, y: 30 },
      {
        opacity: 1,
        scale: 1,
        y: 0,
        duration: 0.6,
        stagger: 0.05,
        ease: 'back.out(1.5)',
        scrollTrigger: {
          trigger: '.tech-grid',
          start: 'top 80%'
        }
      }
    )
  })
  
  function handleMouseMove(e, card) {
    const rect = card.getBoundingClientRect()
    const x = e.clientX - rect.left
    const y = e.clientY - rect.top
    card.style.setProperty('--mouse-x', `${x}px`)
    card.style.setProperty('--mouse-y', `${y}px`)
  }
</script>

<section id="skills" class="skills">
  <div class="container">
    <header class="skills-header">
      <span class="section-label">Stack Tecnológico</span>
      <h2 class="section-title">Tecnologías que<br/>domino</h2>
    </header>
    
    <div class="tech-grid">
      {#each technologies as tech}
        <div 
          class="tech-card {tech.category}"
          on:mousemove={(e) => handleMouseMove(e, e.currentTarget)}
          role="button"
          tabindex="0"
        >
          <div class="card-glow"></div>
          <div class="card-border"></div>
          <span class="tech-name">{tech.name}</span>
          <div class="tech-dot" style="background-color: {tech.color}"></div>
        </div>
      {/each}
    </div>
  </div>
</section>

<style>
  .skills {
    padding: 8rem 2rem;
    background: var(--bg-primary);
    position: relative;
  }

  .container {
    max-width: 1000px;
    margin: 0 auto;
  }

  .skills-header {
    text-align: center;
    margin-bottom: 4rem;
  }

  .section-label {
    display: inline-block;
    font-family: var(--font-mono);
    font-size: 0.75rem;
    color: var(--text-muted);
    text-transform: uppercase;
    letter-spacing: 0.2em;
    margin-bottom: 1rem;
  }

  .section-title {
    font-size: clamp(2.5rem, 6vw, 4rem);
    font-weight: 700;
    letter-spacing: -0.03em;
    line-height: 1.1;
  }

  /* Tech Grid - Bento Style */
  .tech-grid {
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    gap: 1rem;
  }

  .tech-card {
    position: relative;
    background: rgba(255, 255, 255, 0.02);
    border: 1px solid rgba(255, 255, 255, 0.06);
    border-radius: 16px;
    padding: 1.5rem;
    min-height: 120px;
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    overflow: hidden;
    cursor: default;
    transition: transform 0.3s ease, border-color 0.3s ease;
  }

  .tech-card:hover {
    transform: translateY(-4px) scale(1.02);
    border-color: rgba(255, 255, 255, 0.15);
  }

  /* Glow effect on hover */
  .card-glow {
    position: absolute;
    inset: 0;
    opacity: 0;
    transition: opacity 0.3s ease;
    background: radial-gradient(
      400px circle at var(--mouse-x, 50%) var(--mouse-y, 50%),
      rgba(255, 255, 255, 0.06),
      transparent 40%
    );
  }

  .tech-card:hover .card-glow {
    opacity: 1;
  }

  /* Border glow */
  .card-border {
    position: absolute;
    inset: 0;
    border-radius: 16px;
    padding: 1px;
    background: linear-gradient(
      135deg,
      rgba(255, 255, 255, 0.1),
      transparent 50%,
      rgba(255, 255, 255, 0.05)
    );
    -webkit-mask: 
      linear-gradient(#fff 0 0) content-box, 
      linear-gradient(#fff 0 0);
    mask: 
      linear-gradient(#fff 0 0) content-box, 
      linear-gradient(#fff 0 0);
    -webkit-mask-composite: xor;
    mask-composite: exclude;
    opacity: 0;
    transition: opacity 0.3s ease;
  }

  .tech-card:hover .card-border {
    opacity: 1;
  }

  /* Category accent colors */
  .tech-card.frontend:hover {
    border-color: rgba(97, 218, 251, 0.3);
  }
  
  .tech-card.backend:hover {
    border-color: rgba(51, 153, 51, 0.3);
  }
  
  .tech-card.tools:hover {
    border-color: rgba(242, 78, 30, 0.3);
  }

  .tech-name {
    font-size: 1rem;
    font-weight: 500;
    color: var(--text-primary);
    z-index: 1;
  }

  .tech-dot {
    width: 8px;
    height: 8px;
    border-radius: 50%;
    box-shadow: 0 0 12px currentColor;
    z-index: 1;
  }

  /* Featured cards (larger) */
  .tech-card:nth-child(1) {
    grid-column: span 2;
    grid-row: span 2;
    min-height: auto;
  }

  .tech-card:nth-child(1) .tech-name {
    font-size: 2rem;
    font-weight: 700;
  }

  .tech-card:nth-child(6) {
    grid-column: span 2;
  }

  .tech-card:nth-child(10) {
    grid-row: span 2;
  }

  /* Responsive */
  @media (max-width: 900px) {
    .skills {
      padding: 6rem 1.5rem;
    }

    .tech-grid {
      grid-template-columns: repeat(3, 1fr);
    }

    .tech-card:nth-child(1) {
      grid-column: span 2;
      grid-row: span 1;
    }

    .tech-card:nth-child(1) .tech-name {
      font-size: 1.25rem;
    }

    .tech-card:nth-child(6) {
      grid-column: span 1;
    }

    .tech-card:nth-child(10) {
      grid-row: span 1;
    }
  }

  @media (max-width: 600px) {
    .tech-grid {
      grid-template-columns: repeat(2, 1fr);
      gap: 0.75rem;
    }

    .tech-card {
      min-height: 100px;
      padding: 1rem;
    }

    .tech-card:nth-child(1) {
      grid-column: span 2;
    }

    .tech-name {
      font-size: 0.875rem;
    }
  }
</style>
