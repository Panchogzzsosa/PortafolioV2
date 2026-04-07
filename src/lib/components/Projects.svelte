<script>
  import { onMount } from 'svelte'
  import { gsap } from 'gsap'
  import { ScrollTrigger } from 'gsap/ScrollTrigger'
  
  gsap.registerPlugin(ScrollTrigger)
  
  let sectionRef
  
  const projects = [
    {
      id: 1,
      title: 'Worldwide Records',
      description: 'Plataforma de distribución musical digital con panel administrativo completo.',
      category: 'Web Application',
      year: '2024',
      image: '/img/wwr.webp',
      link: 'https://worldwiderecords.com.mx'
    },
    {
      id: 2,
      title: 'FNC VOL 1',
      description: 'Proyecto de recopilación musical - Primer volumen de la serie FNC.',
      category: 'Music Project',
      year: '2024',
      image: '/img/FNCVOL1.webp',
      link: 'https://funnynoiseclub.com/inicio.html'
    },
    {
      id: 3,
      title: 'MAYDAY2K',
      description: 'Proyecto presentacion de album del artista 2k.',
      category: 'Music Project',
      year: '2024',
      image: '/img/Mayday.webp',
      link: 'http://mayday2k.com/'
    },
    {
      id: 4,
      title: 'WUICHOKUN',
      description: 'Proyecto presentacion de album del artista WuichoKun con e-commers.',
      category: 'Music Project',
      year: '2024',
      image: '/img/WuichoKun.webp',
      link: 'https://wuichokun.com.mx/producto.html?id=cd-verde'
    },
    {
      id: 5,
      title: 'Capitan Financiero',
      description: 'Plataforma de educación financiera y gestión de inversiones personales con e-commers.',
      category: 'Web Application',
      year: '2024',
      image: '/img/CapitanFinaciero.webp',
      link: 'https://capitanfinanciero.com/'
    },
    {
      id: 6,
      title: 'Mundial Capitan Financiero',
      description: 'Landing page con registro y correos automaticos para venta de curso financiero.',
      category: 'Landing Page',
      year: '2024',
      image: '/img/MundialCF.webp',
      link: 'https://mundial.capitanfinanciero.com/'
    },
    {
      id: 7,
      title: 'Webinar Capitan Financiero',
      description: 'Landing page con registro para un webinar de capitan financiero con envio de correo automatico.',
      category: 'Landing Page',
      year: '2024',
      image: '/img/WebinaCf.webp',
      link: 'https://webinar.capitanfinanciero.com/'
    },
    {
      id: 8,
      title: 'CityFront AI',
      description: 'Pagina web de empresa cityfront con traduccion a 26 idiomas y deteccion automatica de tu idioma.',
      category: 'Web Application',
      year: '2024',
      image: '/img/CityFront.webp',
      link: 'https://www.cityfront.ai/'
    },
    {
      id: 9,
      title: 'Pymera',
      description: 'Pagina web para empresa Pymera, con formulario completo el cual te dice en donde te puede ayudar la empresa.',
      category: 'Web Application',
      year: '2024',
      image: '/img/Pymera.webp',
      link: 'https://pymera.mx/'
    },
    {
      id: 10,
      title: 'Pymera SaaS',
      description: 'Pagina web para promocion del saas de pymera.',
      category: 'Landing Page',
      year: '2024',
      image: '/img/SaasPymera.webp',
      link: 'https://saas.pymera.mx/'
    },
    {
      id: 11,
      title: 'Granitika SaaS',
      description: 'Saas para granitika junto a pymera.',
      category: 'SaaS',
      year: '2024',
      image: '/img/SaasGranitika.webp',
      link: '#'
    },
    {
      id: 12,
      title: 'CodigoPostal',
      description: 'Pagina web para la empresa codigopostal con dashboard incluido.',
      category: 'Web Application',
      year: '2024',
      image: '/img/CodigoPostal.webp',
      link: 'https://codigopostal.mx/'
    },
    {
      id: 13,
      title: 'Thermal Coatings',
      description: 'Landing page para Thermal Coatings donde ofrecen servicios y productos.',
      category: 'Landing Page',
      year: '2024',
      image: '/img/Thermal Coatings.webp',
      link: 'https://recubrimientosmty.com/'
    },
    {
      id: 14,
      title: 'Jersix',
      description: 'E-commers para tienda de jersey de futbol.',
      category: 'E-commerce',
      year: '2024',
      image: '/img/Jersix.webp',
      link: '#'
    },
    {
      id: 15,
      title: 'aacruzylopez',
      description: 'Landing page para empresa de seguros aacruzylopez con cotizacion de fianzas automatica.',
      category: 'Landing Page',
      year: '2024',
      image: '/img/aacruzylopez.webp',
      link: 'https://www.aacruzylopez.com/fianzas'
    }
  ]
  
  onMount(() => {
    gsap.fromTo('.project-card', 
      { y: 50, opacity: 0 },
      { 
        y: 0, 
        opacity: 1, 
        duration: 0.6,
        stagger: 0.1,
        ease: 'power2.out',
        scrollTrigger: { 
          trigger: '.projects-grid', 
          start: 'top 85%',
          once: true 
        }
      }
    )
  })
</script>

<section bind:this={sectionRef} id="projects" class="projects">
  <div class="container">
    <div class="section-header">
      <h2 class="section-title">Proyectos</h2>
    </div>
    
    <div class="projects-grid">
      {#each projects as project}
        {#if project.link === '#'}
          <div class="project-card disabled">
            <div class="card-image">
              <img src={project.image} alt={project.title} loading="lazy" />
            </div>
            <div class="card-overlay">
              <div class="overlay-content">
                <h3 class="project-title">{project.title}</h3>
                <p class="project-desc">{project.description}</p>
                <span class="project-link private">Privado</span>
              </div>
            </div>
          </div>
        {:else}
          <a href={project.link} target="_blank" rel="noopener" class="project-card">
            <div class="card-image">
              <img src={project.image} alt={project.title} loading="lazy" />
            </div>
            <div class="card-overlay">
              <div class="overlay-content">
                <h3 class="project-title">{project.title}</h3>
                <p class="project-desc">{project.description}</p>
                <span class="project-link">Ver proyecto →</span>
              </div>
            </div>
          </a>
        {/if}
      {/each}
    </div>
  </div>
</section>

<style>
  .projects {
    padding: 8rem 4rem;
    background: var(--bg-primary);
  }

  .container {
    max-width: 1200px;
    margin: 0 auto;
  }

  .section-header {
    margin-bottom: 4rem;
    padding-bottom: 1.5rem;
    border-bottom: 1px solid rgba(255, 255, 255, 0.1);
  }

  .section-title {
    font-size: clamp(2rem, 4vw, 3rem);
    font-weight: 600;
    letter-spacing: -0.02em;
  }

  .projects-grid {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    gap: 1.5rem;
  }

  .project-card {
    display: block;
    position: relative;
    text-decoration: none;
    color: inherit;
    border-radius: 8px;
    overflow: hidden;
    background: linear-gradient(135deg, #1a1a2e 0%, #16213e 100%);
    cursor: pointer;
  }

  .project-card.disabled {
    cursor: default;
  }

  .project-card.disabled .card-overlay {
    background: linear-gradient(
      to top,
      rgba(0, 0, 0, 0.85) 0%,
      rgba(0, 0, 0, 0.6) 50%,
      rgba(0, 0, 0, 0.2) 100%
    );
  }

  .project-link.private {
    color: rgba(255, 255, 255, 0.5);
    font-style: italic;
  }

  .card-image {
    width: 100%;
    line-height: 0;
  }

  .card-image img {
    width: 100%;
    height: auto;
    object-fit: contain;
    transition: transform 0.6s ease;
    display: block;
  }

  .project-card:not(.disabled):hover .card-image img {
    transform: scale(1.05);
  }

  .card-overlay {
    position: absolute;
    inset: 0;
    background: linear-gradient(
      to top,
      rgba(0, 0, 0, 0.95) 0%,
      rgba(0, 0, 0, 0.8) 40%,
      rgba(0, 0, 0, 0.4) 100%
    );
    display: flex;
    align-items: flex-end;
    padding: 1.5rem;
    opacity: 0;
    transition: opacity 0.4s ease;
  }

  .project-card:hover .card-overlay {
    opacity: 1;
  }

  .overlay-content {
    transform: translateY(20px);
    transition: transform 0.4s ease;
  }

  .project-card:hover .overlay-content {
    transform: translateY(0);
  }

  .project-title {
    font-size: 1.25rem;
    font-weight: 600;
    margin-bottom: 0.5rem;
    color: #fff;
  }

  .project-desc {
    font-size: 0.875rem;
    line-height: 1.5;
    color: rgba(255, 255, 255, 0.7);
    margin-bottom: 1rem;
    display: -webkit-box;
    -webkit-line-clamp: 2;
    -webkit-box-orient: vertical;
    overflow: hidden;
  }

  .project-link {
    font-size: 0.875rem;
    font-weight: 500;
    color: #fff;
    display: inline-flex;
    align-items: center;
    gap: 0.25rem;
  }

  @media (max-width: 900px) {
    .projects {
      padding: 6rem 2rem;
    }

    .projects-grid {
      grid-template-columns: repeat(2, 1fr);
      gap: 1rem;
    }
  }

  @media (max-width: 600px) {
    .projects-grid {
      grid-template-columns: 1fr;
    }
  }
</style>
