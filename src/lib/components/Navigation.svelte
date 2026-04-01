<script>
  import { onMount } from 'svelte'
  import { gsap } from 'gsap'
  import { ScrollTrigger } from 'gsap/ScrollTrigger'
  
  gsap.registerPlugin(ScrollTrigger)
  
  let scrolled = $state(false)
  let activeSection = $state('hero')
  let menuOpen = $state(false)
  let navRef
  
  const navItems = [
    { id: 'hero', label: 'Inicio' },
    { id: 'about', label: 'Sobre mí' },
    { id: 'projects', label: 'Proyectos' },
    { id: 'skills', label: 'Habilidades' },
    { id: 'contact', label: 'Contacto' }
  ]
  
  onMount(() => {
    // Entrance animation
    gsap.fromTo(navRef,
      { y: -100, opacity: 0 },
      { y: 0, opacity: 1, duration: 1, ease: 'expo.out', delay: 0.5 }
    )
    
    // Scroll handler
    const handleScroll = () => {
      scrolled = window.scrollY > 100
      
      // Detect active section
      const sections = navItems.map(item => document.getElementById(item.id))
      const scrollPos = window.scrollY + window.innerHeight / 3
      
      for (let i = sections.length - 1; i >= 0; i--) {
        const section = sections[i]
        if (section && section.offsetTop <= scrollPos) {
          activeSection = navItems[i].id
          break
        }
      }
    }
    
    window.addEventListener('scroll', handleScroll, { passive: true })
    handleScroll()
    
    return () => window.removeEventListener('scroll', handleScroll)
  })
  
  function scrollToSection(id) {
    const element = document.getElementById(id)
    if (element) {
      element.scrollIntoView({ behavior: 'smooth' })
      menuOpen = false
    }
  }
</script>

<nav bind:this={navRef} class="navigation" class:scrolled>
  <div class="nav-content">
    <!-- Logo -->
    <a 
      href="#hero" 
      class="logo" 
      onclick={(e) => { e.preventDefault(); scrollToSection('hero'); }}
      data-cursor-hover
    >
      <span class="logo-bracket">[</span>
      <span class="logo-text">Portfolio</span>
      <span class="logo-bracket">]</span>
    </a>
    
    <!-- Desktop Menu -->
    <div class="nav-links">
      {#each navItems as item}
        <a
          href="#{item.id}"
          class="nav-link"
          class:active={activeSection === item.id}
          onclick={(e) => { e.preventDefault(); scrollToSection(item.id); }}
          data-cursor-hover
        >
          <span class="nav-number">0{navItems.indexOf(item) + 1}</span>
          <span class="nav-label">{item.label}</span>
          {#if activeSection === item.id}
            <span class="nav-indicator"></span>
          {/if}
        </a>
      {/each}
    </div>
    
    <!-- Mobile Menu Toggle -->
    <button 
      class="menu-toggle"
      aria-label={menuOpen ? 'Cerrar menú' : 'Abrir menú'}
      onclick={() => menuOpen = !menuOpen}
      data-cursor-hover
    >
      <span class="toggle-line" class:open={menuOpen}></span>
      <span class="toggle-line" class:open={menuOpen}></span>
    </button>
  </div>
  
  <!-- Mobile Menu -->
  {#if menuOpen}
    <div class="mobile-menu" onclick={() => menuOpen = false}>
      <div class="mobile-links" onclick={(e) => e.stopPropagation()}>
        {#each navItems as item, i}
          <a
            href="#{item.id}"
            class="mobile-link"
            class:active={activeSection === item.id}
            style="animation-delay: {i * 50}ms"
            onclick={(e) => { e.preventDefault(); scrollToSection(item.id); }}
          >
            <span class="mobile-number">0{i + 1}</span>
            {item.label}
          </a>
        {/each}
      </div>
    </div>
  {/if}
</nav>

<style>
  .navigation {
    position: fixed;
    top: 0;
    left: 0;
    right: 0;
    z-index: 1000;
    padding: 1.5rem 3rem;
    transition: all 0.5s var(--ease-expo-out);
  }

  .navigation.scrolled {
    padding: 1rem 3rem;
    background-color: rgba(3, 3, 3, 0.8);
    backdrop-filter: blur(20px) saturate(180%);
    border-bottom: 1px solid var(--border);
  }

  .nav-content {
    max-width: 1400px;
    margin: 0 auto;
    display: flex;
    align-items: center;
    justify-content: space-between;
  }

  /* Logo */
  .logo {
    display: flex;
    align-items: center;
    gap: 0.25rem;
    font-family: var(--font-mono);
    font-size: 1rem;
    font-weight: 500;
    color: var(--text-primary);
    text-decoration: none;
    transition: opacity 0.3s ease;
  }

  .logo:hover {
    opacity: 0.7;
  }

  .logo-bracket {
    color: var(--text-muted);
    transition: color 0.3s ease;
  }

  .logo:hover .logo-bracket {
    color: var(--accent);
  }

  /* Nav Links */
  .nav-links {
    display: flex;
    align-items: center;
    gap: 2.5rem;
  }

  .nav-link {
    display: flex;
    align-items: center;
    gap: 0.5rem;
    font-size: 0.875rem;
    color: var(--text-muted);
    text-decoration: none;
    transition: color 0.3s ease;
    position: relative;
    padding: 0.5rem 0;
  }

  .nav-link:hover,
  .nav-link.active {
    color: var(--text-primary);
  }

  .nav-number {
    font-family: var(--font-mono);
    font-size: 0.6875rem;
    color: var(--text-muted);
    transition: color 0.3s ease;
  }

  .nav-link:hover .nav-number,
  .nav-link.active .nav-number {
    color: var(--accent);
  }

  .nav-indicator {
    position: absolute;
    bottom: 0;
    left: 0;
    width: 100%;
    height: 1px;
    background-color: var(--accent);
    animation: expandWidth 0.3s var(--ease-expo-out);
  }

  @keyframes expandWidth {
    from { transform: scaleX(0); }
    to { transform: scaleX(1); }
  }

  /* Menu Toggle */
  .menu-toggle {
    display: none;
    flex-direction: column;
    gap: 6px;
    padding: 0.5rem;
    background: none;
    border: none;
    cursor: pointer;
    z-index: 1001;
  }

  .toggle-line {
    display: block;
    width: 24px;
    height: 1.5px;
    background-color: var(--text-primary);
    transition: all 0.4s var(--ease-expo-out);
  }

  .toggle-line.open:nth-child(1) {
    transform: rotate(45deg) translate(5px, 5px);
  }

  .toggle-line.open:nth-child(2) {
    transform: rotate(-45deg) translate(0, 0);
  }

  /* Mobile Menu */
  .mobile-menu {
    display: none;
    position: fixed;
    inset: 0;
    background-color: rgba(0, 0, 0, 0.95);
    backdrop-filter: blur(30px);
    z-index: 999;
    animation: fadeIn 0.4s ease;
  }

  @keyframes fadeIn {
    from { opacity: 0; }
    to { opacity: 1; }
  }

  .mobile-links {
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    display: flex;
    flex-direction: column;
    gap: 1rem;
    text-align: center;
  }

  .mobile-link {
    font-size: 2.5rem;
    font-weight: 300;
    color: var(--text-primary);
    text-decoration: none;
    opacity: 0;
    transform: translateY(30px);
    animation: slideUp 0.5s var(--ease-expo-out) forwards;
    display: flex;
    align-items: center;
    justify-content: center;
    gap: 1rem;
    padding: 0.5rem 0;
  }

  @keyframes slideUp {
    to {
      opacity: 1;
      transform: translateY(0);
    }
  }

  .mobile-link.active {
    color: var(--accent);
  }

  .mobile-number {
    font-family: var(--font-mono);
    font-size: 0.875rem;
    color: var(--text-muted);
  }

  /* Responsive */
  @media (max-width: 768px) {
    .navigation {
      padding: 1rem 1.5rem;
    }

    .nav-links {
      display: none;
    }

    .menu-toggle {
      display: flex;
    }

    .mobile-menu {
      display: flex;
    }
  }
</style>
