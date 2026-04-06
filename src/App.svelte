<script>
  import { onMount } from 'svelte'
  import { gsap } from 'gsap'
  import { ScrollTrigger } from 'gsap/ScrollTrigger'
  
  // Components
  import Preloader from './lib/components/Preloader.svelte'
  import SmoothScroll from './lib/components/SmoothScroll.svelte'
  import Navigation from './lib/components/Navigation.svelte'
  import Hero from './lib/components/Hero.svelte'
  import About from './lib/components/About.svelte'
  import Projects from './lib/components/Projects.svelte'
  import Skills from './lib/components/Skills.svelte'
  import Contact from './lib/components/Contact.svelte'
  
  gsap.registerPlugin(ScrollTrigger)
  
  // State
  let isLoading = $state(true)
  
  function handlePreloaderComplete() {
    isLoading = false
    // Refresh ScrollTrigger after preloader
    setTimeout(() => {
      ScrollTrigger.refresh()
    }, 100)
  }
  

  

</script>

{#if isLoading}
  <Preloader onComplete={handlePreloaderComplete} />
{/if}

<SmoothScroll />
<Navigation />

<main>
  <Hero />
  
  <About />
  
  <Projects />
  
  <Skills />
  
  <Contact />
</main>

<style>
  :global(*) {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
  }

  :global(:root) {
    /* Colors - Premium dark */
    --bg-primary: #030303;
    --bg-secondary: #080808;
    --bg-tertiary: #0f0f0f;
    --bg-glass: rgba(255, 255, 255, 0.02);
    
    --text-primary: #ffffff;
    --text-secondary: rgba(255, 255, 255, 0.65);
    --text-muted: rgba(255, 255, 255, 0.35);
    
    --accent: #ffffff;
    --accent-dim: rgba(255, 255, 255, 0.5);
    
    --border: rgba(255, 255, 255, 0.06);
    --border-hover: rgba(255, 255, 255, 0.15);
    
    /* Typography */
    --font-sans: 'Inter', system-ui, sans-serif;
    --font-mono: 'JetBrains Mono', monospace;
    
    /* Easing - Premium */
    --ease-smooth: cubic-bezier(0.4, 0, 0.2, 1);
    --ease-expo-out: cubic-bezier(0.16, 1, 0.3, 1);
    --ease-expo-in: cubic-bezier(0.7, 0, 0.84, 0);
    --ease-elastic: cubic-bezier(0.34, 1.56, 0.64, 1);
    
    /* Durations */
    --duration-instant: 100ms;
    --duration-fast: 250ms;
    --duration-normal: 500ms;
    --duration-slow: 800ms;
    
    /* Spacing */
    --space-xs: 0.5rem;
    --space-sm: 1rem;
    --space-md: 1.5rem;
    --space-lg: 2.5rem;
    --space-xl: 4rem;
    --space-2xl: 6rem;
    --space-3xl: 10rem;
  }

  :global(html) {
    scroll-behavior: auto; /* Lenis handles this */
  }

  :global(body) {
    font-family: var(--font-sans);
    background-color: var(--bg-primary);
    color: var(--text-primary);
    line-height: 1.6;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    overflow-x: hidden;
  }

  :global(::selection) {
    background-color: var(--accent);
    color: var(--bg-primary);
  }

  :global(::-webkit-scrollbar) {
    width: 8px;
  }

  :global(::-webkit-scrollbar-track) {
    background: var(--bg-primary);
  }

  :global(::-webkit-scrollbar-thumb) {
    background: var(--border-hover);
    border-radius: 4px;
  }

  :global(::-webkit-scrollbar-thumb:hover) {
    background: var(--text-muted);
  }

  main {
    position: relative;
  }

  /* Grain texture overlay */
  :global(body::before) {
    content: '';
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    pointer-events: none;
    z-index: 9999;
    opacity: 0.025;
    background-image: url("data:image/svg+xml,%3Csvg viewBox='0 0 200 200' xmlns='http://www.w3.org/2000/svg'%3E%3Cfilter id='noise'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='0.65' numOctaves='3' stitchTiles='stitch'/%3E%3C/filter%3E%3Crect width='100%25' height='100%25' filter='url(%23noise)'/%3E%3C/svg%3E");
  }

  /* Reduced motion */
  @media (prefers-reduced-motion: reduce) {
    :global(*) {
      animation-duration: 0.01ms !important;
      transition-duration: 0.01ms !important;
    }
  }
</style>
