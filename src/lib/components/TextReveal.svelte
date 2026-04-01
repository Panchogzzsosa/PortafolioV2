<script>
  import { onMount } from 'svelte'
  import { gsap } from 'gsap'
  import { ScrollTrigger } from 'gsap/ScrollTrigger'
  
  gsap.registerPlugin(ScrollTrigger)
  
  export let text = ''
  export let className = ''
  export let delay = 0
  
  let containerRef
  let chars = text.split('')
  
  onMount(() => {
    const triggers = []
    
    const chars = containerRef.querySelectorAll('.char')
    
    const tl = gsap.timeline({
      scrollTrigger: {
        trigger: containerRef,
        start: 'top 80%',
        end: 'top 20%',
        scrub: 1,
      }
    })
    
    tl.fromTo(chars, 
      {
        opacity: 0.1,
        y: 50,
        rotateX: -90,
      },
      {
        opacity: 1,
        y: 0,
        rotateX: 0,
        stagger: 0.02,
        ease: 'power2.out'
      }
    )
    
    if (tl.scrollTrigger) {
      triggers.push(tl.scrollTrigger)
    }
    
    return () => {
      triggers.forEach(t => t.kill())
    }
  })
</script>

<span bind:this={containerRef} class="text-reveal {className}">
  {#each chars as char, i}
    <span class="char" style="display: inline-block;">
      {char === ' ' ? '\u00A0' : char}
    </span>
  {/each}
</span>

<style>
  .text-reveal {
    display: inline-block;
    perspective: 1000px;
  }
  
  .char {
    display: inline-block;
    transform-origin: center bottom;
    will-change: transform, opacity;
  }
</style>
