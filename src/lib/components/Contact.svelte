<script>
  import { onMount } from 'svelte'
  import { gsap } from 'gsap'
  import { ScrollTrigger } from 'gsap/ScrollTrigger'
  
  gsap.registerPlugin(ScrollTrigger)
  
  let sectionRef
  
  const socialLinks = [
    { name: 'GitHub', url: 'https://github.com/Panchogzzsosa', handle: '@Panchogzzsosa' },
    { name: 'LinkedIn', url: 'https://www.linkedin.com/in/francisco-miguel-gonz%C3%A1lez-sosa-13a46430b/', handle: 'Francisco Miguel González Sosa' },
    { name: 'WhatsApp', url: 'https://wa.me/8117220005', handle: '+52 81 1722 0005' },
    { name: 'Email', url: 'mailto:franciscogzz03@gmail.com', handle: 'franciscogzz03@gmail.com' }
  ]
  
  onMount(() => {
    const triggers = []
    
    // Main content animation
    gsap.fromTo('.contact-main > *',
      { opacity: 0, y: 50 },
      {
        opacity: 1,
        y: 0,
        duration: 0.8,
        stagger: 0.1,
        ease: 'expo.out',
        scrollTrigger: {
          trigger: '.contact-main',
          start: 'top 80%',
          toggleActions: 'play none none reverse'
        }
      }
    )
    
    // Social links stagger
    gsap.fromTo('.social-link',
      { opacity: 0, x: 50 },
      {
        opacity: 1,
        x: 0,
        duration: 0.6,
        stagger: 0.1,
        ease: 'expo.out',
        scrollTrigger: {
          trigger: '.contact-links',
          start: 'top 80%',
          toggleActions: 'play none none reverse'
        }
      }
    )
    
    // Magnetic effect on social links
    const links = document.querySelectorAll('.social-link')
    links.forEach(link => {
      link.addEventListener('mousemove', (e) => {
        const rect = link.getBoundingClientRect()
        const x = e.clientX - rect.left - rect.width / 2
        const y = e.clientY - rect.top - rect.height / 2
        
        gsap.to(link, {
          x: x * 0.2,
          y: y * 0.2,
          duration: 0.3,
          ease: 'power2.out'
        })
      })
      
      link.addEventListener('mouseleave', () => {
        gsap.to(link, {
          x: 0,
          y: 0,
          duration: 0.5,
          ease: 'elastic.out(1, 0.3)'
        })
      })
    })
    
    return () => {
      triggers.forEach(t => t.kill())
    }
  })
</script>

<section bind:this={sectionRef} id="contact" class="contact">
  <div class="contact-content">
    <div class="contact-main">
      <span class="section-tag">Contacto</span>
      
      <h2 class="contact-title">
        ¿Tienes un proyecto
        <span class="title-line">en mente?</span>
      </h2>
      
      <p class="contact-description">
      </p>
      
      <a 
        href="https://wa.me/8117220005" 
        class="contact-email"
        data-cursor-hover
      >
        <span class="email-text">+52 81 1722 00005</span>
        <span class="email-arrow">→</span>
      </a>
    </div>
    
    <div class="contact-links">
      {#each socialLinks as link, i}
        <a 
          href={link.url}
          class="social-link"
          target="_blank"
          rel="noopener noreferrer"
          data-cursor-hover
        >
          <div class="link-content">
            <span class="link-name">{link.name}</span>
            <span class="link-handle">{link.handle}</span>
          </div>
          <svg class="link-arrow" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
            <path d="M7 17L17 7M17 7H7M17 7V17" stroke-linecap="round" stroke-linejoin="round"/>
          </svg>
        </a>
      {/each}
    </div>
  </div>
  
  <footer class="footer">
    <div class="footer-line"></div>
    <div class="footer-content">
      <p class="footer-text">
        <span class="footer-year">© {new Date().getFullYear()}</span>
        <span class="footer-divider">—</span>
        <span>Diseñado y desarrollado con precisión</span>
      </p>

    </div>
  </footer>
</section>

<style>
  .contact {
    padding: var(--space-3xl) 2rem 0;
    position: relative;
    min-height: 100vh;
    display: flex;
    flex-direction: column;
  }

  .contact-content {
    max-width: 1200px;
    margin: 0 auto;
    width: 100%;
    flex: 1;
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 6rem;
    align-items: center;
    padding: 4rem 0;
  }

  /* Main content */
  .contact-main {
    display: flex;
    flex-direction: column;
    gap: 2rem;
  }

  .section-tag {
    display: inline-flex;
    align-items: center;
    gap: 0.75rem;
    font-family: var(--font-mono);
    font-size: 0.8125rem;
    color: var(--text-muted);
    text-transform: uppercase;
    letter-spacing: 0.1em;
  }

  .section-tag::before {
    content: '';
    width: 40px;
    height: 1px;
    background-color: var(--accent);
  }

  .contact-title {
    font-size: clamp(2.5rem, 5vw, 4.5rem);
    font-weight: 700;
    line-height: 1.1;
    letter-spacing: -0.03em;
  }

  .title-line {
    display: block;
    color: var(--text-secondary);
  }

  .contact-description {
    font-size: 1.125rem;
    color: var(--text-secondary);
    max-width: 400px;
    line-height: 1.8;
  }

  .contact-email {
    display: inline-flex;
    align-items: center;
    gap: 1rem;
    margin-top: 1rem;
    font-size: 1.75rem;
    color: var(--accent);
    text-decoration: none;
    position: relative;
    transition: gap 0.3s var(--ease-expo-out);
  }

  .contact-email:hover {
    gap: 1.5rem;
  }

  .email-text {
    position: relative;
  }

  .email-text::after {
    content: '';
    position: absolute;
    bottom: -4px;
    left: 0;
    width: 0;
    height: 2px;
    background-color: var(--accent);
    transition: width 0.4s var(--ease-expo-out);
  }

  .contact-email:hover .email-text::after {
    width: 100%;
  }

  .email-arrow {
    transition: transform 0.4s var(--ease-expo-out);
  }

  .contact-email:hover .email-arrow {
    transform: translate(12px, -12px);
  }

  /* Links */
  .contact-links {
    display: flex;
    flex-direction: column;
    gap: 1px;
    background-color: var(--border);
    border-radius: 20px;
    overflow: hidden;
  }

  .social-link {
    display: flex;
    align-items: center;
    justify-content: space-between;
    padding: 1.75rem 2rem;
    background-color: var(--bg-secondary);
    text-decoration: none;
    color: var(--text-primary);
    transition: all 0.4s var(--ease-expo-out);
    will-change: transform;
  }

  .social-link:hover {
    background-color: var(--bg-tertiary);
    padding-left: 2.5rem;
    padding-right: 1.5rem;
  }

  .link-content {
    display: flex;
    flex-direction: column;
    gap: 0.25rem;
  }

  .link-name {
    font-size: 1.125rem;
    font-weight: 500;
  }

  .link-handle {
    font-family: var(--font-mono);
    font-size: 0.8125rem;
    color: var(--text-muted);
  }

  .link-arrow {
    width: 24px;
    height: 24px;
    opacity: 0;
    transform: translate(-10px, 10px);
    transition: all 0.3s var(--ease-expo-out);
  }

  .social-link:hover .link-arrow {
    opacity: 1;
    transform: translate(0, 0);
  }

  /* Footer */
  .footer {
    margin-top: auto;
  }

  .footer-line {
    height: 1px;
    background: linear-gradient(90deg, transparent, var(--border), transparent);
  }

  .footer-content {
    max-width: 1200px;
    margin: 0 auto;
    padding: 2rem;
    display: flex;
    justify-content: space-between;
    align-items: center;
    flex-wrap: wrap;
    gap: 1rem;
  }

  .footer-text {
    display: flex;
    align-items: center;
    gap: 0.75rem;
    font-size: 0.875rem;
    color: var(--text-muted);
  }

  .footer-year {
    font-family: var(--font-mono);
  }

  .footer-divider {
    opacity: 0.5;
  }

  /* Responsive */
  @media (max-width: 968px) {
    .contact {
      padding: var(--space-2xl) 1.5rem 0;
    }

    .contact-content {
      grid-template-columns: 1fr;
      gap: 4rem;
      padding: 2rem 0;
    }

    .contact-links {
      order: -1;
    }

    .contact-email {
      font-size: 1.25rem;
    }

    .footer-content {
      flex-direction: column;
      text-align: center;
      padding: 1.5rem;
    }
  }
</style>
