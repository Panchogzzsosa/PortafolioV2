<script>
  import { onMount } from 'svelte'
  import { gsap } from 'gsap'
  
  let cursorX = $state(0)
  let cursorY = $state(0)
  let dotX = $state(0)
  let dotY = $state(0)
  let isHovering = $state(false)
  let isVisible = $state(false)
  let isClicking = $state(false)
  
  onMount(() => {
    // Check if touch device
    if (window.matchMedia('(pointer: coarse)').matches) {
      return
    }
    
    let rafId
    
    const updateCursor = (e) => {
      cursorX = e.clientX
      cursorY = e.clientY
      isVisible = true
      
      // Smooth follow for dot
      const animate = () => {
        const dx = cursorX - dotX
        const dy = cursorY - dotY
        
        dotX += dx * 0.12
        dotY += dy * 0.12
        
        rafId = requestAnimationFrame(animate)
      }
      
      if (!rafId) {
        rafId = requestAnimationFrame(animate)
      }
    }
    
    const handleMouseEnter = () => isVisible = true
    const handleMouseLeave = () => isVisible = false
    
    const handleMouseDown = () => {
      isClicking = true
      gsap.to('.cursor-ring', { scale: 0.8, duration: 0.15 })
    }
    
    const handleMouseUp = () => {
      isClicking = false
      gsap.to('.cursor-ring', { scale: 1, duration: 0.3, ease: 'elastic.out(1, 0.3)' })
    }
    
    // Hover detection with MutationObserver
    const setupHoverListeners = () => {
      const hoverElements = document.querySelectorAll('a, button, [data-cursor-hover], input, textarea')
      hoverElements.forEach(el => {
        el.addEventListener('mouseenter', () => isHovering = true)
        el.addEventListener('mouseleave', () => isHovering = false)
      })
    }
    
    window.addEventListener('mousemove', updateCursor, { passive: true })
    document.addEventListener('mouseenter', handleMouseEnter)
    document.addEventListener('mouseleave', handleMouseLeave)
    document.addEventListener('mousedown', handleMouseDown)
    document.addEventListener('mouseup', handleMouseUp)
    
    // Re-add listeners when DOM changes
    const observer = new MutationObserver(setupHoverListeners)
    observer.observe(document.body, { childList: true, subtree: true })
    
    // Initial setup
    setTimeout(setupHoverListeners, 100)
    
    return () => {
      window.removeEventListener('mousemove', updateCursor)
      document.removeEventListener('mouseenter', handleMouseEnter)
      document.removeEventListener('mouseleave', handleMouseLeave)
      document.removeEventListener('mousedown', handleMouseDown)
      document.removeEventListener('mouseup', handleMouseUp)
      if (rafId) cancelAnimationFrame(rafId)
      observer.disconnect()
    }
  })
</script>

{#if isVisible}
  <!-- Main cursor ring -->
  <div 
    class="cursor-ring"
    class:hovering={isHovering}
    class:clicking={isClicking}
    style="transform: translate({cursorX}px, {cursorY}px) translate(-50%, -50%)"
  ></div>
  
  <!-- Cursor dot -->
  <div 
    class="cursor-dot"
    class:hovering={isHovering}
    style="transform: translate({dotX}px, {dotY}px) translate(-50%, -50%)"
  ></div>
{/if}

<style>
  .cursor-ring {
    position: fixed;
    top: 0;
    left: 0;
    width: 48px;
    height: 48px;
    border: 1px solid rgba(255, 255, 255, 0.4);
    border-radius: 50%;
    pointer-events: none;
    z-index: 10000;
    transition: 
      width 0.4s var(--ease-expo-out),
      height 0.4s var(--ease-expo-out),
      border-color 0.3s ease,
      background-color 0.3s ease;
    will-change: transform;
  }

  .cursor-ring.hovering {
    width: 64px;
    height: 64px;
    border-color: rgba(255, 255, 255, 0.8);
    background-color: rgba(255, 255, 255, 0.03);
    mix-blend-mode: difference;
  }

  .cursor-ring.clicking {
    transform: translate(var(--x, 0), var(--y, 0)) translate(-50%, -50%) scale(0.8) !important;
  }

  .cursor-dot {
    position: fixed;
    top: 0;
    left: 0;
    width: 6px;
    height: 6px;
    background-color: white;
    border-radius: 50%;
    pointer-events: none;
    z-index: 10001;
    transition: 
      width 0.3s var(--ease-elastic),
      height 0.3s var(--ease-elastic),
      opacity 0.2s ease;
    will-change: transform;
  }

  .cursor-dot.hovering {
    width: 0;
    height: 0;
    opacity: 0;
  }

  /* Hide on touch devices */
  @media (pointer: coarse) {
    .cursor-ring,
    .cursor-dot {
      display: none;
    }
  }
</style>
