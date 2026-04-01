<script>
  import { onMount } from 'svelte'
  
  let visible = $state(false)
  let sectionRef
  let hoveredProject = $state(null)
  
  const projects = [
    {
      id: 1,
      title: 'E-commerce Premium',
      category: 'Web Development',
      year: '2024',
      description: 'Plataforma de comercio electrónico con experiencia de usuario excepcional.',
      tags: ['Next.js', 'Stripe', 'PostgreSQL'],
      color: '#8b5cf6'
    },
    {
      id: 2,
      title: 'Dashboard Analytics',
      category: 'Data Visualization',
      year: '2024',
      description: 'Panel de control en tiempo real con visualizaciones interactivas.',
      tags: ['React', 'D3.js', 'Node.js'],
      color: '#06b6d4'
    },
    {
      id: 3,
      title: 'App Mobile Fintech',
      category: 'Mobile Development',
      year: '2023',
      description: 'Aplicación bancaria con enfoque en seguridad y usabilidad.',
      tags: ['React Native', 'Firebase', 'TypeScript'],
      color: '#f59e0b'
    },
    {
      id: 4,
      title: 'Plataforma SaaS',
      category: 'Full Stack',
      year: '2023',
      description: 'Herramienta de gestión de equipos con colaboración en tiempo real.',
      tags: ['SvelteKit', 'Prisma', 'Redis'],
      color: '#ec4899'
    }
  ]
  
  onMount(() => {
    const observer = new IntersectionObserver(
      ([entry]) => {
        if (entry.isIntersecting) {
          visible = true
          observer.disconnect()
        }
      },
      { threshold: 0.1 }
    )
    
    if (sectionRef) observer.observe(sectionRef)
    return () => observer.disconnect()
  })
</script>

<section id="projects" class="projects" bind:this={sectionRef}>
  <div class="projects-content">
    <header class="projects-header">
      <span class="section-tag" class:visible>Proyectos destacados</span>
      <h2 class="projects-title" class:visible style="transition-delay: 100ms">
        Trabajos seleccionados
      </h2>
      <p class="projects-subtitle" class:visible style="transition-delay: 200ms">
        Una colección curada de proyectos que representan mi visión creativa
      </p>
    </header>
    
    <div class="projects-list">
      {#each projects as project, i}
        <article 
          class="project-item"
          class:visible
          class:hovered={hoveredProject === project.id}
          style="transition-delay: {300 + i * 100}ms"
          onmouseenter={() => hoveredProject = project.id}
          onmouseleave={() => hoveredProject = null}
        >
          <div class="project-main">
            <div class="project-meta">
              <span class="project-number">0{i + 1}</span>
              <span class="project-year">{project.year}</span>
            </div>
            
            <div class="project-info">
              <span class="project-category">{project.category}</span>
              <h3 class="project-title">{project.title}</h3>
              <p class="project-description">{project.description}</p>
            </div>
            
            <div class="project-action">
              <button class="view-btn" aria-label="Ver proyecto">
                <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                  <path d="M7 17L17 7M17 7H7M17 7V17" stroke-linecap="round" stroke-linejoin="round"/>
                </svg>
              </button>
            </div>
          </div>
          
          <div class="project-visual" style="--project-color: {project.color}">
            <div class="visual-placeholder">
              <div class="placeholder-glow"></div>
              <span class="placeholder-text">{project.title}</span>
            </div>
          </div>
          
          <div class="project-tags">
            {#each project.tags as tag}
              <span class="tag">{tag}</span>
            {/each}
          </div>
        </article>
      {/each}
    </div>
    
    <div class="projects-footer" class:visible style="transition-delay: 800ms">
      <a href="#contact" class="more-link">
        <span>¿Tienes un proyecto en mente?</span>
        <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
          <path d="M5 12h14M12 5l7 7-7 7" stroke-linecap="round" stroke-linejoin="round"/>
        </svg>
      </a>
    </div>
  </div>
</section>

<style>
  .projects {
    padding: var(--space-3xl) 2rem;
    position: relative;
  }

  .projects-content {
    max-width: 1200px;
    margin: 0 auto;
  }

  /* Header */
  .projects-header {
    margin-bottom: 5rem;
    text-align: center;
  }

  .section-tag {
    display: inline-block;
    font-family: var(--font-mono);
    font-size: 0.875rem;
    color: var(--text-muted);
    text-transform: uppercase;
    letter-spacing: 0.2em;
    margin-bottom: 1rem;
    opacity: 0;
    transform: translateY(20px);
    transition: all 0.8s var(--ease-expo-out);
  }

  .section-tag.visible {
    opacity: 1;
    transform: translateY(0);
  }

  .projects-title {
    font-size: clamp(2.5rem, 5vw, 4rem);
    font-weight: 700;
    letter-spacing: -0.03em;
    margin-bottom: 1rem;
    opacity: 0;
    transform: translateY(30px);
    transition: all 0.8s var(--ease-expo-out);
  }

  .projects-title.visible {
    opacity: 1;
    transform: translateY(0);
  }

  .projects-subtitle {
    font-size: 1.125rem;
    color: var(--text-secondary);
    max-width: 500px;
    margin: 0 auto;
    opacity: 0;
    transform: translateY(20px);
    transition: all 0.8s var(--ease-expo-out);
  }

  .projects-subtitle.visible {
    opacity: 1;
    transform: translateY(0);
  }

  /* Projects list */
  .projects-list {
    display: flex;
    flex-direction: column;
    gap: 1px;
    background-color: var(--border);
    border-radius: 24px;
    overflow: hidden;
  }

  .project-item {
    background-color: var(--bg-secondary);
    padding: 3rem;
    position: relative;
    overflow: hidden;
    opacity: 0;
    transform: translateY(30px);
    transition: all 0.8s var(--ease-expo-out);
    cursor: pointer;
  }

  .project-item.visible {
    opacity: 1;
    transform: translateY(0);
  }

  .project-item::before {
    content: '';
    position: absolute;
    inset: 0;
    background: radial-gradient(
      600px circle at var(--mouse-x, 50%) var(--mouse-y, 50%),
      rgba(255, 255, 255, 0.03),
      transparent 40%
    );
    opacity: 0;
    transition: opacity 0.3s ease;
    pointer-events: none;
  }

  .project-item:hover::before {
    opacity: 1;
  }

  .project-main {
    display: grid;
    grid-template-columns: auto 1fr auto;
    gap: 3rem;
    align-items: start;
    position: relative;
    z-index: 1;
  }

  .project-meta {
    display: flex;
    flex-direction: column;
    gap: 0.5rem;
  }

  .project-number {
    font-family: var(--font-mono);
    font-size: 0.875rem;
    color: var(--text-muted);
  }

  .project-year {
    font-family: var(--font-mono);
    font-size: 0.75rem;
    color: var(--text-muted);
    opacity: 0.6;
  }

  .project-info {
    display: flex;
    flex-direction: column;
    gap: 0.75rem;
  }

  .project-category {
    font-size: 0.75rem;
    text-transform: uppercase;
    letter-spacing: 0.15em;
    color: var(--text-muted);
  }

  .project-title {
    font-size: clamp(1.5rem, 3vw, 2rem);
    font-weight: 600;
    transition: color 0.3s ease;
  }

  .project-item:hover .project-title {
    color: var(--project-color, var(--accent));
  }

  .project-description {
    font-size: 0.9375rem;
    color: var(--text-secondary);
    max-width: 400px;
    line-height: 1.6;
  }

  .project-action {
    display: flex;
    align-items: center;
  }

  .view-btn {
    width: 56px;
    height: 56px;
    border-radius: 50%;
    border: 1px solid var(--border);
    background: transparent;
    color: var(--text-primary);
    display: flex;
    align-items: center;
    justify-content: center;
    cursor: pointer;
    transition: all 0.4s var(--ease-expo-out);
  }

  .view-btn svg {
    width: 20px;
    height: 20px;
    transition: transform 0.3s var(--ease-expo-out);
  }

  .project-item:hover .view-btn {
    background-color: var(--accent);
    border-color: var(--accent);
    color: var(--bg-primary);
    transform: rotate(-45deg);
  }

  .project-item:hover .view-btn svg {
    transform: rotate(45deg);
  }

  /* Visual */
  .project-visual {
    position: absolute;
    top: 50%;
    right: 15%;
    transform: translateY(-50%) translateX(50px);
    width: 300px;
    height: 200px;
    opacity: 0;
    transition: all 0.6s var(--ease-expo-out);
    pointer-events: none;
    z-index: 0;
  }

  .project-item:hover .project-visual {
    opacity: 1;
    transform: translateY(-50%) translateX(0);
  }

  .visual-placeholder {
    width: 100%;
    height: 100%;
    background: linear-gradient(135deg, var(--bg-tertiary), var(--bg-secondary));
    border-radius: 16px;
    border: 1px solid var(--border);
    display: flex;
    align-items: center;
    justify-content: center;
    position: relative;
    overflow: hidden;
  }

  .placeholder-glow {
    position: absolute;
    width: 150%;
    height: 150%;
    background: radial-gradient(circle, var(--project-color, #fff) 0%, transparent 70%);
    opacity: 0.1;
    filter: blur(40px);
  }

  .placeholder-text {
    font-size: 0.875rem;
    color: var(--text-muted);
    z-index: 1;
  }

  /* Tags */
  .project-tags {
    display: flex;
    gap: 0.5rem;
    margin-top: 1.5rem;
    padding-left: calc(60px + 3rem);
    flex-wrap: wrap;
  }

  .tag {
    padding: 0.375rem 0.75rem;
    background-color: var(--bg-tertiary);
    border: 1px solid var(--border);
    border-radius: 100px;
    font-size: 0.75rem;
    font-family: var(--font-mono);
    color: var(--text-muted);
    transition: all 0.3s ease;
  }

  .project-item:hover .tag {
    border-color: var(--border-hover);
  }

  /* Footer */
  .projects-footer {
    margin-top: 4rem;
    text-align: center;
    opacity: 0;
    transform: translateY(20px);
    transition: all 0.8s var(--ease-expo-out);
  }

  .projects-footer.visible {
    opacity: 1;
    transform: translateY(0);
  }

  .more-link {
    display: inline-flex;
    align-items: center;
    gap: 0.75rem;
    font-size: 1.125rem;
    color: var(--text-secondary);
    text-decoration: none;
    transition: color 0.3s ease;
  }

  .more-link:hover {
    color: var(--accent);
  }

  .more-link svg {
    width: 20px;
    height: 20px;
    transition: transform 0.3s var(--ease-expo-out);
  }

  .more-link:hover svg {
    transform: translateX(4px);
  }

  /* Responsive */
  @media (max-width: 968px) {
    .projects {
      padding: var(--space-2xl) 1.5rem;
    }

    .project-item {
      padding: 2rem;
    }

    .project-main {
      grid-template-columns: 1fr auto;
      gap: 1.5rem;
    }

    .project-meta {
      display: none;
    }

    .project-visual {
      display: none;
    }

    .project-tags {
      padding-left: 0;
    }
  }
</style>
