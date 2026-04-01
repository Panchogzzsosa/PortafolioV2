<script>
  import { onMount } from 'svelte'
  import { gsap } from 'gsap'
  
  let { onComplete } = $props()
  
  let preloaderRef
  let counterRef
  let contentRef
  let progress = $state(0)
  let isExiting = $state(false)
  
  onMount(() => {
    const tl = gsap.timeline()
    
    // Counter animation 0-100
    const counterObj = { value: 0 }
    tl.to(counterObj, {
      value: 100,
      duration: 2.5,
      ease: 'power2.inOut',
      onUpdate: () => {
        progress = Math.round(counterObj.value)
      }
    })
    
    // Text reveal animation
    tl.fromTo('.preloader-text .char', 
      { y: 100, opacity: 0 },
      { 
        y: 0, 
        opacity: 1, 
        duration: 0.8, 
        stagger: 0.05,
        ease: 'expo.out'
      },
      0.2
    )
    
    // Progress bar
    tl.fromTo('.progress-line',
      { scaleX: 0 },
      { scaleX: 1, duration: 2.5, ease: 'power2.inOut' },
      0
    )
    
    // Exit animation
    tl.add(() => { isExiting = true }, 2.8)
    
    // Split screen exit
    tl.to('.preloader-top', {
      yPercent: -100,
      duration: 1,
      ease: 'expo.inOut'
    }, 2.8)
    
    tl.to('.preloader-bottom', {
      yPercent: 100,
      duration: 1,
      ease: 'expo.inOut'
    }, 2.8)
    
    tl.to('.preloader-content', {
      opacity: 0,
      duration: 0.5,
      ease: 'power2.in'
    }, 2.8)
    
    // Call complete callback
    tl.add(() => {
      onComplete()
    }, 3.5)
    
    return () => {
      tl.kill()
    }
  })
  
  const text = 'PORTAFOLIO'
</script>

<div bind:this={preloaderRef} class="preloader">
  <!-- Split background panels -->
  <div class="preloader-top"></div>
  <div class="preloader-bottom"></div>
  
  <!-- Center content -->
  <div bind:this={contentRef} class="preloader-content">
    <!-- Main text -->
    <div class="preloader-text">
      {#each text.split('') as char, i}
        <span class="char">{char}</span>
      {/each}
    </div>
    
    <!-- Counter -->
    <div class="preloader-counter">
      <span class="counter-number" bind:this={counterRef}>{progress}</span>
      <span class="counter-suffix">%</span>
    </div>
    
    <!-- Progress bar -->
    <div class="progress-container">
      <div class="progress-line"></div>
    </div>
    
    <!-- Loading text -->
    <div class="loading-text">
      <span class="loading-dots">Cargando experiencia</span>
    </div>
  </div>
  
  <!-- Corner decorations -->
  <div class="corner corner-tl"></div>
  <div class="corner corner-tr"></div>
  <div class="corner corner-bl"></div>
  <div class="corner corner-br"></div>
</div>

<style>
  .preloader {
    position: fixed;
    inset: 0;
    z-index: 99999;
    display: flex;
    align-items: center;
    justify-content: center;
    overflow: hidden;
  }

  .preloader-top,
  .preloader-bottom {
    position: absolute;
    left: 0;
    right: 0;
    height: 50%;
    background-color: var(--bg-primary);
    will-change: transform;
  }

  .preloader-top {
    top: 0;
    border-bottom: 1px solid var(--border);
  }

  .preloader-bottom {
    bottom: 0;
    border-top: 1px solid var(--border);
  }

  .preloader-content {
    position: relative;
    z-index: 10;
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 2rem;
  }

  /* Main text */
  .preloader-text {
    font-size: clamp(2.5rem, 10vw, 8rem);
    font-weight: 700;
    letter-spacing: 0.15em;
    display: flex;
    overflow: hidden;
    text-align: center;
  }

  .char {
    display: inline-block;
    will-change: transform, opacity;
    background: linear-gradient(135deg, #fff 0%, rgba(255,255,255,0.6) 100%);
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
    background-clip: text;
  }

  /* Counter */
  .preloader-counter {
    display: flex;
    align-items: baseline;
    gap: 0.25rem;
    font-family: var(--font-mono);
    font-size: 1rem;
    color: var(--text-muted);
  }

  .counter-number {
    font-size: 1.5rem;
    color: var(--text-primary);
    min-width: 3ch;
    text-align: right;
  }

  /* Progress bar */
  .progress-container {
    width: 200px;
    height: 1px;
    background-color: var(--border);
    overflow: hidden;
  }

  .progress-line {
    width: 100%;
    height: 100%;
    background-color: var(--accent);
    transform-origin: left;
    will-change: transform;
  }

  /* Loading text */
  .loading-text {
    font-family: var(--font-mono);
    font-size: 0.75rem;
    letter-spacing: 0.3em;
    text-transform: uppercase;
    color: var(--text-muted);
  }

  .loading-dots::after {
    content: '';
    animation: dots 1.5s steps(4, end) infinite;
  }

  @keyframes dots {
    0% { content: ''; }
    25% { content: '.'; }
    50% { content: '..'; }
    75% { content: '...'; }
    100% { content: ''; }
  }

  /* Corner decorations */
  .corner {
    position: absolute;
    width: 40px;
    height: 40px;
    border: 1px solid var(--border);
    z-index: 20;
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
    .preloader-text {
      letter-spacing: 0.2em;
    }

    .corner {
      width: 24px;
      height: 24px;
    }
  }
</style>
