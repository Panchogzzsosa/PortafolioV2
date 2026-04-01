<script>
  import { onMount } from 'svelte'
  import { gsap } from 'gsap'
  import { ScrollTrigger } from 'gsap/ScrollTrigger'
  
  gsap.registerPlugin(ScrollTrigger)
  
  export let items = []
  
  let sectionRef
  let containerRef
  let trackRef
  
  onMount(() => {
    const triggers = []
    
    const track = trackRef
    const cards = track.querySelectorAll('.horizontal-card')
    
    // Calculate total scroll distance
    const totalWidth = track.scrollWidth - window.innerWidth
    
    // Horizontal scroll animation
    const tl = gsap.timeline({
      scrollTrigger: {
        trigger: sectionRef,
        start: 'top top',
        end: () => `+=${totalWidth}`,
        scrub: 1,
        pin: true,
        anticipatePin: 1,
        invalidateOnRefresh: true,
      }
    })
    
    tl.to(track, {
      x: -totalWidth,
      ease: 'none'
    })
    
    // Animate cards as they enter viewport
    cards.forEach((card, i) => {
      gsap.fromTo(card,
        { 
          opacity: 0.3,
          scale: 0.9,
          y: 50
        },
        {
          opacity: 1,
          scale: 1,
          y: 0,
          duration: 0.5,
          scrollTrigger: {
            trigger: card,
            containerAnimation: tl,
            start: 'left 80%',
            end: 'left 50%',
            scrub: true,
          }
        }
      )
    })
    
    if (tl.scrollTrigger) {
      triggers.push(tl.scrollTrigger)
    }
    
    return () => {
      triggers.forEach(t => t.kill())
    }
  })
</script>

<section bind:this={sectionRef} class="horizontal-section">
  <div class="horizontal-header">
    <span class="section-label">Galería</span>
    <h2 class="section-title">Explora los detalles</h2>
  </div>
  
  <div bind:this={containerRef} class="horizontal-container">
    <div bind:this={trackRef} class="horizontal-track">
      {#each items as item, i}
        <div class="horizontal-card">
          <div class="card-visual">
            <div class="visual-placeholder" style="--card-color: {item.color}">
              <div class="placeholder-glow"></div>
              <span class="placeholder-icon">{item.icon || '◆'}</span>
            </div>
          </div>
          <div class="card-content">
            <span class="card-number">0{i + 1}</span>
            <h3 class="card-title">{item.title}</h3>
            <p class="card-text">{item.description}</p>
          </div>
        </div>
      {/each}
      
      <!-- End card -->
      <div class="horizontal-card end-card">
        <div class="end-content">
          <h3>¿Listo para crear algo increíble?</h3>
          <a href="#contact" class="end-cta">Hablemos →</a>
        </div>
      </div>
    </div>
  </div>
  
  <div class="scroll-hint">
    <div class="hint-line"></div>
    <span>Scroll horizontal</span>
  </div>
</section>

<style>
  .horizontal-section {
    position: relative;
    height: 100vh;
    background-color: var(--bg-primary);
    overflow: hidden;
  }
  
  .horizontal-header {
    position: absolute;
    top: 3rem;
    left: 3rem;
    z-index: 10;
  }
  
  .section-label {
    display: block;
    font-family: var(--font-mono);
    font-size: 0.875rem;
    color: var(--text-muted);
    text-transform: uppercase;
    letter-spacing: 0.2em;
    margin-bottom: 0.5rem;
  }
  
  .section-title {
    font-size: 2rem;
    font-weight: 600;
  }
  
  .horizontal-container {
    position: relative;
    height: 100%;
    display: flex;
    align-items: center;
  }
  
  .horizontal-track {
    display: flex;
    gap: 3rem;
    padding: 0 10vw;
    will-change: transform;
  }
  
  .horizontal-card {
    flex-shrink: 0;
    width: 500px;
    height: 600px;
    background: linear-gradient(135deg, var(--bg-tertiary), var(--bg-secondary));
    border: 1px solid var(--border);
    border-radius: 32px;
    overflow: hidden;
    display: flex;
    flex-direction: column;
    will-change: transform, opacity;
  }
  
  .card-visual {
    height: 60%;
    position: relative;
  }
  
  .visual-placeholder {
    width: 100%;
    height: 100%;
    background: linear-gradient(135deg, var(--bg-tertiary), var(--bg-secondary));
    display: flex;
    align-items: center;
    justify-content: center;
    position: relative;
    overflow: hidden;
  }
  
  .placeholder-glow {
    position: absolute;
    width: 200%;
    height: 200%;
    background: radial-gradient(circle, var(--card-color, #fff) 0%, transparent 60%);
    opacity: 0.1;
    filter: blur(60px);
  }
  
  .placeholder-icon {
    font-size: 4rem;
    color: var(--text-muted);
    z-index: 1;
  }
  
  .card-content {
    flex: 1;
    padding: 2rem;
    display: flex;
    flex-direction: column;
    gap: 0.75rem;
  }
  
  .card-number {
    font-family: var(--font-mono);
    font-size: 0.875rem;
    color: var(--accent);
  }
  
  .card-title {
    font-size: 1.75rem;
    font-weight: 600;
  }
  
  .card-text {
    font-size: 0.9375rem;
    color: var(--text-secondary);
    line-height: 1.6;
  }
  
  /* End card */
  .end-card {
    width: 600px;
    display: flex;
    align-items: center;
    justify-content: center;
    text-align: center;
    background: linear-gradient(135deg, var(--accent), var(--bg-tertiary));
  }
  
  .end-content {
    padding: 3rem;
  }
  
  .end-content h3 {
    font-size: 2.5rem;
    font-weight: 700;
    margin-bottom: 2rem;
    color: var(--bg-primary);
  }
  
  .end-cta {
    display: inline-flex;
    padding: 1rem 2rem;
    background-color: var(--bg-primary);
    color: var(--accent);
    text-decoration: none;
    border-radius: 100px;
    font-weight: 500;
    transition: transform 0.3s var(--ease-expo-out);
  }
  
  .end-cta:hover {
    transform: scale(1.05);
  }
  
  /* Scroll hint */
  .scroll-hint {
    position: absolute;
    bottom: 3rem;
    right: 3rem;
    display: flex;
    align-items: center;
    gap: 1rem;
    font-family: var(--font-mono);
    font-size: 0.75rem;
    color: var(--text-muted);
    text-transform: uppercase;
    letter-spacing: 0.1em;
  }
  
  .hint-line {
    width: 60px;
    height: 1px;
    background-color: var(--text-muted);
    position: relative;
    overflow: hidden;
  }
  
  .hint-line::after {
    content: '';
    position: absolute;
    top: 0;
    left: -100%;
    width: 100%;
    height: 100%;
    background: linear-gradient(90deg, transparent, var(--accent), transparent);
    animation: shimmer 2s infinite;
  }
  
  @keyframes shimmer {
    to { left: 100%; }
  }
</style>
