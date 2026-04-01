<script>
  import { onMount } from 'svelte'
  import { gsap } from 'gsap'
  import { ScrollTrigger } from 'gsap/ScrollTrigger'
  
  gsap.registerPlugin(ScrollTrigger)
  
  export let src = ''
  export let alt = ''
  export let speed = 0.5
  
  let containerRef
  let imageRef
  
  onMount(() => {
    const triggers = []
    
    // Parallax effect
    const st = ScrollTrigger.create({
      trigger: containerRef,
      start: 'top bottom',
      end: 'bottom top',
      scrub: true,
      onUpdate: (self) => {
        if (imageRef) {
          const yPos = self.progress * 100 * speed
          gsap.set(imageRef, { y: yPos - 50 * speed })
        }
      }
    })
    
    triggers.push(st)
    
    return () => {
      triggers.forEach(t => t.kill())
    }
  })
</script>

<div bind:this={containerRef} class="parallax-container">
  <div bind:this={imageRef} class="parallax-image-wrapper">
    <div class="image-placeholder">
      <span class="placeholder-text">{alt}</span>
    </div>
  </div>
</div>

<style>
  .parallax-container {
    position: relative;
    width: 100%;
    height: 100%;
    overflow: hidden;
    border-radius: 16px;
  }
  
  .parallax-image-wrapper {
    position: absolute;
    inset: -20%;
    will-change: transform;
  }
  
  .image-placeholder {
    width: 100%;
    height: 100%;
    background: linear-gradient(135deg, var(--bg-tertiary), var(--bg-secondary));
    display: flex;
    align-items: center;
    justify-content: center;
    border: 1px solid var(--border);
  }
  
  .placeholder-text {
    font-size: 0.875rem;
    color: var(--text-muted);
  }
</style>
