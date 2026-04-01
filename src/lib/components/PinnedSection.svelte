<script>
  import { onMount } from 'svelte'
  import { gsap } from 'gsap'
  import { ScrollTrigger } from 'gsap/ScrollTrigger'
  
  gsap.registerPlugin(ScrollTrigger)
  
  export let title = ''
  export let subtitle = ''
  export let items = []
  
  let sectionRef
  let containerRef
  
  onMount(() => {
    const triggers = []
    
    // Pin the section and animate through items
    const cards = containerRef.querySelectorAll('.pinned-card')
    
    const tl = gsap.timeline({
      scrollTrigger: {
        trigger: sectionRef,
        start: 'top top',
        end: `+=${items.length * 100}%`,
        scrub: 1,
        pin: true,
        anticipatePin: 1,
      }
    })
    
    cards.forEach((card, i) => {
      if (i === 0) {
        tl.fromTo(card, 
          { opacity: 0, y: 100, scale: 0.8 },
          { opacity: 1, y: 0, scale: 1, duration: 0.5 }
        )
      } else {
        tl.to(cards[i - 1], {
          opacity: 0,
          y: -100,
          scale: 0.8,
          duration: 0.5
        })
        .fromTo(card,
          { opacity: 0, y: 100, scale: 0.8 },
          { opacity: 1, y: 0, scale: 1, duration: 0.5 },
          '<'
        )
      }
    })
    
    if (tl.scrollTrigger) {
      triggers.push(tl.scrollTrigger)
    }
    
    return () => {
      triggers.forEach(t => t.kill())
    }
  })
</script>

<section bind:this={sectionRef} class="pinned-section">
  <div class="pinned-header">
    <span class="pinned-label">{subtitle}</span>
    <h2 class="pinned-title">{title}</h2>
  </div>
  
  <div bind:this={containerRef} class="pinned-container">
    {#each items as item, i}
      <div class="pinned-card" style="z-index: {items.length - i}">
        <div class="card-number">0{i + 1}</div>
        <h3 class="card-title">{item.title}</h3>
        <p class="card-description">{item.description}</p>
        <div class="card-tags">
          {#each item.tags as tag}
            <span class="tag">{tag}</span>
          {/each}
        </div>
      </div>
    {/each}
  </div>
  
  <div class="progress-indicator">
    {#each items as _, i}
      <div class="progress-dot"></div>
    {/each}
  </div>
</section>

<style>
  .pinned-section {
    position: relative;
    height: 100vh;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    padding: 2rem;
    background-color: var(--bg-primary);
  }
  
  .pinned-header {
    text-align: center;
    margin-bottom: 4rem;
  }
  
  .pinned-label {
    display: block;
    font-family: var(--font-mono);
    font-size: 0.875rem;
    color: var(--text-muted);
    text-transform: uppercase;
    letter-spacing: 0.2em;
    margin-bottom: 1rem;
  }
  
  .pinned-title {
    font-size: clamp(2rem, 5vw, 4rem);
    font-weight: 700;
    letter-spacing: -0.03em;
  }
  
  .pinned-container {
    position: relative;
    width: 100%;
    max-width: 600px;
    height: 400px;
  }
  
  .pinned-card {
    position: absolute;
    inset: 0;
    background: linear-gradient(135deg, var(--bg-tertiary), var(--bg-secondary));
    border: 1px solid var(--border);
    border-radius: 24px;
    padding: 3rem;
    display: flex;
    flex-direction: column;
    justify-content: center;
    will-change: transform, opacity;
  }
  
  .card-number {
    font-family: var(--font-mono);
    font-size: 0.875rem;
    color: var(--accent);
    margin-bottom: 1rem;
  }
  
  .card-title {
    font-size: 2rem;
    font-weight: 600;
    margin-bottom: 1rem;
  }
  
  .card-description {
    font-size: 1rem;
    color: var(--text-secondary);
    line-height: 1.7;
    margin-bottom: 2rem;
  }
  
  .card-tags {
    display: flex;
    gap: 0.5rem;
    flex-wrap: wrap;
  }
  
  .tag {
    padding: 0.5rem 1rem;
    background-color: var(--bg-secondary);
    border: 1px solid var(--border);
    border-radius: 100px;
    font-size: 0.75rem;
    font-family: var(--font-mono);
    color: var(--text-muted);
  }
  
  .progress-indicator {
    position: absolute;
    bottom: 3rem;
    left: 50%;
    transform: translateX(-50%);
    display: flex;
    gap: 0.75rem;
  }
  
  .progress-dot {
    width: 8px;
    height: 8px;
    border-radius: 50%;
    background-color: var(--border);
    transition: background-color 0.3s ease;
  }
  
  .progress-dot.active {
    background-color: var(--accent);
  }
</style>
