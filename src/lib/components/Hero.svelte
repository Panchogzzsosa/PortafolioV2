<script>
  import { onMount } from 'svelte'
  import { gsap } from 'gsap'
  import { ScrollTrigger } from 'gsap/ScrollTrigger'
  
  gsap.registerPlugin(ScrollTrigger)
  
  let heroRef
  let titleRef
  let subtitleRef
  let ctaRef
  
  // Scramble effect configuration
  const chars = '!<>-_\\/[]{}—=+*^?#________'
  const finalTitle = 'DESARROLLADOR'
  const finalSubtitle = 'Full Stack'
  
  let displayTitle = $state('')
  let displaySubtitle = $state('')
  let showCursor = $state(true)
  let isScrambling = $state(true)
  
  onMount(() => {
    // Scramble text effect
    const scrambleText = (finalText, setDisplay, duration = 2000) => {
      return new Promise((resolve) => {
        let iteration = 0
        const totalIterations = finalText.length * 3
        const interval = duration / totalIterations
        
        const intervalId = setInterval(() => {
          setDisplay(
            finalText
              .split('')
              .map((char, index) => {
                if (index < iteration / 3) {
                  return finalText[index]
                }
                return chars[Math.floor(Math.random() * chars.length)]
              })
              .join('')
          )
          
          iteration++
          
          if (iteration >= totalIterations) {
            clearInterval(intervalId)
            setDisplay(finalText)
            resolve()
          }
        }, interval)
      })
    }
    
    // Run scramble effects
    const runAnimations = async () => {
      await scrambleText(finalTitle, (text) => displayTitle = text, 2500)
      await new Promise(r => setTimeout(r, 300))
      await scrambleText(finalSubtitle, (text) => displaySubtitle = text, 1500)
      isScrambling = false
      
      // Animate other elements after text scramble
      gsap.fromTo('.hero-description',
        { opacity: 0, y: 30 },
        { opacity: 1, y: 0, duration: 0.8, ease: 'expo.out' }
      )
      
      gsap.fromTo('.hero-actions',
        { opacity: 0, y: 30 },
        { opacity: 1, y: 0, duration: 0.8, ease: 'expo.out', delay: 0.2 }
      )
      
      gsap.fromTo('.hero-meta',
        { opacity: 0 },
        { opacity: 1, duration: 1, delay: 0.5 }
      )
    }
    
    runAnimations()
    
    // Cursor blink
    const cursorInterval = setInterval(() => {
      showCursor = !showCursor
    }, 530)
    
    // Parallax on scroll
    const st = ScrollTrigger.create({
      trigger: heroRef,
      start: 'top top',
      end: 'bottom top',
      scrub: true,
      onUpdate: (self) => {
        const progress = self.progress
        gsap.set(titleRef, { y: progress * 100 })
      }
    })
    
    return () => {
      clearInterval(cursorInterval)
      st.kill()
    }
  })
</script>

<section bind:this={heroRef} id="hero" class="hero">
  <!-- Animated code lines background -->
  <div class="code-lines">
    {#each Array(20) as _, i}
      <div class="code-line" style="--delay: {i * 0.1}s; --left: {Math.random() * 100}%">
        <span class="code-text">
          {Array(30).fill(0).map(() => chars[Math.floor(Math.random() * chars.length)]).join('')}
        </span>
      </div>
    {/each}
  </div>
  
  <!-- Grid overlay -->
  <div class="grid-overlay"></div>
  
  <!-- Scan line effect -->
  <div class="scan-line"></div>
  
  <div class="hero-content">
    <!-- Meta info -->
    <div class="hero-meta">
      <span class="meta-item">FRANCISCO_GONZALEZ_SOSA</span>
      <span class="meta-divider">//</span>
      <span class="meta-item">2026</span>
    </div>
    
    <!-- Main title with scramble effect -->
    <h1 bind:this={titleRef} class="hero-title">
      <span class="title-line">
        <span class="scramble-text">{displayTitle}</span>
        <span class="cursor" class:blink={showCursor && !isScrambling}>_</span>
      </span>
      <span class="subtitle-line">
        <span class="scramble-text scramble-secondary">{displaySubtitle}</span>
      </span>

    </h1>
    
    <!-- Description -->
    <p class="hero-description">
      Diseño y desarrollo soluciones tecnológicas escalables con enfoque en 
      performance, usabilidad y arquitectura limpia.
    </p>
    
    <!-- CTA Buttons -->
    <div bind:this={ctaRef} class="hero-actions">
      <a href="#projects" class="btn btn-primary">
        <span class="btn-bracket">[</span>
        <span class="btn-text">VER_PROYECTOS</span>
        <span class="btn-bracket">]</span>
      </a>
      <a href="#contact" class="btn btn-secondary">
        <span class="btn-bracket">[</span>
        <span class="btn-text">CONTACTAR</span>
        <span class="btn-bracket">]</span>
      </a>
    </div>
  </div>
  
  <div class="scroll-indicator">
    <span class="scroll-text">SCROLL</span>
    <span class="scroll-arrow">↓</span>
  </div>
  
  <!-- Glitch overlay -->
  <div class="glitch-overlay"></div>
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
    background: linear-gradient(180deg, var(--bg-primary) 0%, #0a0a0f 100%);
  }

  /* Code lines background */
  .code-lines {
    position: absolute;
    inset: 0;
    overflow: hidden;
    pointer-events: none;
  }

  .code-line {
    position: absolute;
    left: var(--left);
    top: -100%;
    font-family: var(--font-mono);
    font-size: 0.875rem;
    color: rgba(255, 255, 255, 0.25);
    white-space: nowrap;
    animation: fall 10s linear infinite;
    animation-delay: var(--delay);
    writing-mode: vertical-lr;
    text-orientation: upright;
    letter-spacing: 0.3em;
  }

  @keyframes fall {
    0% {
      top: -20%;
      opacity: 0;
    }
    10% {
      opacity: 1;
    }
    90% {
      opacity: 1;
    }
    100% {
      top: 120%;
      opacity: 0;
    }
  }

  /* Grid overlay */
  .grid-overlay {
    position: absolute;
    inset: 0;
    background-image: 
      linear-gradient(rgba(255, 255, 255, 0.02) 1px, transparent 1px),
      linear-gradient(90deg, rgba(255, 255, 255, 0.02) 1px, transparent 1px);
    background-size: 50px 50px;
    mask-image: radial-gradient(ellipse at center, black 40%, transparent 80%);
    pointer-events: none;
  }

  /* Scan line */
  .scan-line {
    position: absolute;
    left: 0;
    right: 0;
    height: 2px;
    background: linear-gradient(90deg, 
      transparent, 
      rgba(255, 255, 255, 0.1), 
      transparent
    );
    animation: scan 4s linear infinite;
    pointer-events: none;
  }

  @keyframes scan {
    0% { top: -10%; }
    100% { top: 110%; }
  }

  .hero-content {
    position: relative;
    z-index: 10;
    max-width: 1200px;
    text-align: center;
  }

  /* Meta info */
  .hero-meta {
    display: flex;
    align-items: center;
    justify-content: center;
    gap: 1rem;
    margin-bottom: 3rem;
    font-family: var(--font-mono);
    font-size: 0.75rem;
    letter-spacing: 0.3em;
    color: var(--text-muted);
    opacity: 0;
  }

  .meta-divider {
    color: var(--border-hover);
  }

  /* Title */
  .hero-title {
    font-size: clamp(2.5rem, 8vw, 6rem);
    font-weight: 700;
    line-height: 1.1;
    letter-spacing: -0.02em;
    margin-bottom: 2rem;
  }

  .title-line,
  .subtitle-line {
    display: block;
    position: relative;
  }

  .scramble-text {
    font-family: var(--font-mono);
    letter-spacing: 0.1em;
  }

  .scramble-secondary {
    font-size: 0.6em;
    color: var(--text-secondary);
    font-weight: 300;
    letter-spacing: 0.3em;
  }

  .cursor {
    display: inline-block;
    color: var(--accent);
    animation: none;
  }

  .cursor.blink {
    animation: blink 1s step-end infinite;
  }

  .cursor-secondary {
    color: var(--text-muted);
    font-size: 0.6em;
  }

  @keyframes blink {
    50% { opacity: 0; }
  }

  /* Description */
  .hero-description {
    font-size: 1.125rem;
    color: var(--text-secondary);
    max-width: 600px;
    margin: 0 auto 3rem;
    line-height: 1.8;
    opacity: 0;
  }

  /* Actions */
  .hero-actions {
    display: flex;
    gap: 1.5rem;
    justify-content: center;
    flex-wrap: wrap;
    opacity: 0;
  }

  .btn {
    display: inline-flex;
    align-items: center;
    gap: 0.5rem;
    padding: 1rem 2rem;
    font-family: var(--font-mono);
    font-size: 0.875rem;
    letter-spacing: 0.15em;
    text-decoration: none;
    border: 1px solid var(--border);
    transition: all 0.3s ease;
    position: relative;
    overflow: hidden;
  }

  .btn::before {
    content: '';
    position: absolute;
    inset: 0;
    background: var(--accent);
    transform: scaleX(0);
    transform-origin: right;
    transition: transform 0.4s var(--ease-expo-out);
    z-index: -1;
  }

  .btn:hover::before {
    transform: scaleX(1);
    transform-origin: left;
  }

  .btn-primary {
    color: var(--text-primary);
  }

  .btn-primary:hover {
    color: var(--bg-primary);
    border-color: var(--accent);
  }

  .btn-secondary {
    color: var(--text-muted);
    background: transparent;
  }

  .btn-secondary:hover {
    color: var(--bg-primary);
    border-color: var(--text-secondary);
  }

  .btn-secondary:hover::before {
    background: var(--text-secondary);
  }

  .btn-bracket {
    color: var(--text-muted);
    transition: color 0.3s ease;
  }

  .btn:hover .btn-bracket {
    color: inherit;
  }

  /* Corner info */
  .corner-info {
    position: absolute;
    font-family: var(--font-mono);
    font-size: 0.6875rem;
    letter-spacing: 0.2em;
    color: var(--text-muted);
    display: flex;
    align-items: center;
    gap: 0.5rem;
  }

  .top-left {
    top: 2rem;
    left: 2rem;
  }

  .top-right {
    top: 2rem;
    right: 2rem;
  }

  .bottom-left {
    bottom: 2rem;
    left: 2rem;
  }

  .bottom-right {
    bottom: 2rem;
    right: 2rem;
  }

  .info-label {
    color: var(--text-muted);
  }

  .info-value {
    color: var(--text-secondary);
  }

  @keyframes pulse {
    0%, 100% { opacity: 1; }
    50% { opacity: 0.5; }
  }

  .scroll-indicator {
    position: absolute;
    bottom: 2rem;
    left: 50%;
    transform: translateX(-50%);
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 0.5rem;
    font-family: var(--font-mono);
    font-size: 0.6875rem;
    letter-spacing: 0.2em;
    color: var(--text-muted);
    cursor: pointer;
    transition: color 0.3s ease;
  }

  .scroll-indicator:hover {
    color: var(--accent);
  }

  .scroll-arrow {
    animation: bounce 2s ease-in-out infinite;
  }

  @keyframes bounce {
    0%, 100% { transform: translateY(0); }
    50% { transform: translateY(4px); }
  }

  /* Glitch effect */
  .glitch-overlay {
    position: absolute;
    inset: 0;
    background: linear-gradient(
      transparent 50%,
      rgba(255, 255, 255, 0.02) 50%
    );
    background-size: 100% 4px;
    pointer-events: none;
    opacity: 0.3;
  }

  /* Responsive */
  @media (max-width: 768px) {
    .hero {
      padding: 6rem 1.5rem;
    }

    .hero-meta {
      flex-direction: column;
      gap: 0.5rem;
    }

    .meta-divider {
      display: none;
    }

    .corner-info {
      display: none;
    }

    .hero-actions {
      flex-direction: column;
    }

    .btn {
      width: 100%;
      justify-content: center;
    }
  }
</style>
