<script>
  import { onMount } from 'svelte'
  import { gsap } from 'gsap'
  import { ScrollTrigger } from 'gsap/ScrollTrigger'
  
  gsap.registerPlugin(ScrollTrigger)
  
  let sectionRef
  
  const skillCategories = [
    {
      title: 'Frontend',
      skills: [
        { name: 'React / Next.js', level: 95 },
        { name: 'Svelte / SvelteKit', level: 90 },
        { name: 'TypeScript', level: 92 },
        { name: 'Tailwind CSS', level: 95 },
        { name: 'Three.js / WebGL', level: 75 }
      ]
    },
    {
      title: 'Backend',
      skills: [
        { name: 'Node.js', level: 90 },
        { name: 'Python / Django', level: 85 },
        { name: 'PostgreSQL', level: 88 },
        { name: 'GraphQL', level: 82 },
        { name: 'Redis', level: 78 }
      ]
    },
    {
      title: 'DevOps & Tools',
      skills: [
        { name: 'Docker', level: 85 },
        { name: 'AWS / Vercel', level: 80 },
        { name: 'Git / CI/CD', level: 90 },
        { name: 'Linux', level: 85 },
        { name: 'Figma', level: 88 }
      ]
    }
  ]
  
  onMount(() => {
    const triggers = []
    
    // Header animation
    gsap.fromTo('.skills-header > *',
      { opacity: 0, y: 40 },
      {
        opacity: 1,
        y: 0,
        duration: 0.8,
        stagger: 0.1,
        ease: 'expo.out',
        scrollTrigger: {
          trigger: '.skills-header',
          start: 'top 80%',
          toggleActions: 'play none none reverse'
        }
      }
    )
    
    // Cards stagger animation
    gsap.fromTo('.category-card',
      { opacity: 0, y: 80, rotateY: -15 },
      {
        opacity: 1,
        y: 0,
        rotateY: 0,
        duration: 1,
        stagger: 0.15,
        ease: 'expo.out',
        scrollTrigger: {
          trigger: '.skills-grid',
          start: 'top 75%',
          toggleActions: 'play none none reverse'
        }
      }
    )
    
    // Skill bars animation
    const bars = document.querySelectorAll('.skill-progress')
    bars.forEach((bar, i) => {
      const width = bar.style.getPropertyValue('--skill-level')
      gsap.fromTo(bar,
        { scaleX: 0 },
        {
          scaleX: 1,
          duration: 1.2,
          ease: 'expo.out',
          scrollTrigger: {
            trigger: bar,
            start: 'top 85%',
            toggleActions: 'play none none reverse'
          },
          delay: i * 0.05
        }
      )
    })
    
    return () => {
      triggers.forEach(t => t.kill())
    }
  })
</script>

<section bind:this={sectionRef} id="skills" class="skills">
  <div class="skills-content">
    <header class="skills-header">
      <span class="section-tag">Habilidades</span>
      <h2 class="skills-title">Tecnologías que domino</h2>
      <p class="skills-subtitle">
        Stack moderno para crear aplicaciones escalables y de alto rendimiento
      </p>
    </header>
    
    <div class="skills-grid">
      {#each skillCategories as category, i}
        <div class="category-card" style="--card-index: {i}">
          <div class="card-shine"></div>
          <h3 class="category-title">{category.title}</h3>
          
          <div class="skills-list">
            {#each category.skills as skill, j}
              <div class="skill-item">
                <div class="skill-header">
                  <span class="skill-name">{skill.name}</span>
                  <span class="skill-percentage">{skill.level}%</span>
                </div>
                <div class="skill-bar">
                  <div 
                    class="skill-progress" 
                    style="--skill-level: {skill.level}%"
                  ></div>
                </div>
              </div>
            {/each}
          </div>
        </div>
      {/each}
    </div>
  </div>
  
  <!-- Decorative orbs -->
  <div class="glow-orb orb-1"></div>
  <div class="glow-orb orb-2"></div>
  <div class="glow-orb orb-3"></div>
</section>

<style>
  .skills {
    padding: var(--space-3xl) 2rem;
    position: relative;
    overflow: hidden;
  }

  .skills-content {
    max-width: 1200px;
    margin: 0 auto;
    position: relative;
    z-index: 1;
  }

  /* Header */
  .skills-header {
    text-align: center;
    margin-bottom: 5rem;
  }

  .section-tag {
    display: inline-block;
    font-family: var(--font-mono);
    font-size: 0.8125rem;
    color: var(--text-muted);
    text-transform: uppercase;
    letter-spacing: 0.2em;
    margin-bottom: 1rem;
  }

  .skills-title {
    font-size: clamp(2.5rem, 5vw, 4rem);
    font-weight: 700;
    letter-spacing: -0.03em;
    margin-bottom: 1rem;
  }

  .skills-subtitle {
    font-size: 1.125rem;
    color: var(--text-secondary);
    max-width: 500px;
    margin: 0 auto;
  }

  /* Skills Grid */
  .skills-grid {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    gap: 2rem;
    perspective: 1000px;
  }

  .category-card {
    position: relative;
    background-color: var(--bg-tertiary);
    border: 1px solid var(--border);
    border-radius: 24px;
    padding: 2.5rem;
    transform-style: preserve-3d;
    transition: all 0.5s var(--ease-expo-out);
    overflow: hidden;
  }

  .category-card:hover {
    border-color: var(--border-hover);
    transform: translateY(-8px) rotateX(5deg);
    box-shadow: 
      0 30px 60px rgba(0, 0, 0, 0.4),
      0 0 0 1px rgba(255, 255, 255, 0.05);
  }

  .card-shine {
    position: absolute;
    top: 0;
    left: -100%;
    width: 100%;
    height: 100%;
    background: linear-gradient(
      90deg,
      transparent,
      rgba(255, 255, 255, 0.03),
      transparent
    );
    transition: left 0.7s ease;
  }

  .category-card:hover .card-shine {
    left: 100%;
  }

  .category-title {
    font-size: 1.25rem;
    font-weight: 600;
    margin-bottom: 2rem;
    padding-bottom: 1rem;
    border-bottom: 1px solid var(--border);
    position: relative;
    z-index: 1;
  }

  .skills-list {
    display: flex;
    flex-direction: column;
    gap: 1.5rem;
    position: relative;
    z-index: 1;
  }

  .skill-item {
    display: flex;
    flex-direction: column;
    gap: 0.5rem;
  }

  .skill-header {
    display: flex;
    justify-content: space-between;
    align-items: center;
  }

  .skill-name {
    font-size: 0.9375rem;
    color: var(--text-secondary);
  }

  .skill-percentage {
    font-family: var(--font-mono);
    font-size: 0.75rem;
    color: var(--text-muted);
  }

  .skill-bar {
    height: 3px;
    background-color: var(--bg-secondary);
    border-radius: 2px;
    overflow: hidden;
  }

  .skill-progress {
    height: 100%;
    width: var(--skill-level);
    background: linear-gradient(90deg, var(--text-secondary), var(--accent));
    border-radius: 2px;
    transform-origin: left;
    will-change: transform;
  }

  /* Glow orbs */
  .glow-orb {
    position: absolute;
    border-radius: 50%;
    filter: blur(80px);
    pointer-events: none;
    opacity: 0.4;
  }

  .orb-1 {
    width: 500px;
    height: 500px;
    background: radial-gradient(circle, rgba(255,255,255,0.03) 0%, transparent 70%);
    top: 10%;
    left: -10%;
    animation: float 20s ease-in-out infinite;
  }

  .orb-2 {
    width: 400px;
    height: 400px;
    background: radial-gradient(circle, rgba(255,255,255,0.02) 0%, transparent 70%);
    bottom: 20%;
    right: -5%;
    animation: float 25s ease-in-out infinite reverse;
  }

  .orb-3 {
    width: 300px;
    height: 300px;
    background: radial-gradient(circle, rgba(255,255,255,0.02) 0%, transparent 70%);
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    animation: pulse 15s ease-in-out infinite;
  }

  @keyframes float {
    0%, 100% { transform: translate(0, 0); }
    25% { transform: translate(30px, -30px); }
    50% { transform: translate(-20px, 20px); }
    75% { transform: translate(20px, 30px); }
  }

  @keyframes pulse {
    0%, 100% { transform: translate(-50%, -50%) scale(1); opacity: 0.4; }
    50% { transform: translate(-50%, -50%) scale(1.2); opacity: 0.2; }
  }

  /* Responsive */
  @media (max-width: 968px) {
    .skills {
      padding: var(--space-2xl) 1.5rem;
    }

    .skills-grid {
      grid-template-columns: 1fr;
    }

    .category-card {
      padding: 2rem;
    }

    .glow-orb {
      display: none;
    }
  }
</style>
