<script>
  import { onMount } from 'svelte';
  import CommandPalette from '$lib/components/CommandPalette.svelte';
  import ParticleField from '$lib/components/ParticleField.svelte';
  import ProjectCard from '$lib/components/ProjectCard.svelte';
  import { projects, skills } from '$lib/data/projects.js';

  let paletteOpen = false;
  let theme = 'dark';
  let activeCategory = 'All';
  let sent = false;
  let form = { name: '', email: '', message: '' };
  let errors = {};

  const categories = ['All', ...new Set(projects.map((project) => project.category))];
  $: visibleProjects =
    activeCategory === 'All'
      ? projects
      : projects.filter((project) => project.category === activeCategory);

  onMount(() => {
    const savedTheme = localStorage.getItem('portfolio-theme');
    if (savedTheme) theme = savedTheme;
    document.documentElement.dataset.theme = theme;

    const keydown = (event) => {
      if ((event.ctrlKey || event.metaKey) && event.key.toLowerCase() === 'k') {
        event.preventDefault();
        paletteOpen = true;
      }
      if (event.key === 'Escape') paletteOpen = false;
    };
    window.addEventListener('keydown', keydown);
    return () => window.removeEventListener('keydown', keydown);
  });

  function toggleTheme() {
    theme = theme === 'dark' ? 'light' : 'dark';
    document.documentElement.dataset.theme = theme;
    localStorage.setItem('portfolio-theme', theme);
  }

  function sanitize(value) {
    return value.replace(/[<>]/g, '').trim();
  }

  function submitContact() {
    const payload = {
      name: sanitize(form.name),
      email: sanitize(form.email),
      message: sanitize(form.message)
    };
    errors = {};
    if (payload.name.length < 2) errors.name = 'Enter your name.';
    if (!/^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(payload.email)) errors.email = 'Use a valid email.';
    if (Object.keys(errors).length) return;

    sent = true;
    const subject = encodeURIComponent(`Portfolio inquiry from ${payload.name}`);
    const message = payload.message || 'Hello Chizaram, I would like to connect with you.';
    const body = encodeURIComponent(`${message}\n\nReply to: ${payload.email}`);
    const gmailUrl = `https://mail.google.com/mail/?view=cm&fs=1&to=juliuschizaramonyinyechukwu@gmail.com&su=${subject}&body=${body}`;
    window.open(gmailUrl, '_blank', 'noopener,noreferrer');
  }

</script>

<svelte:head>
  <title>Miss Julius Chizaram Onyinyechukwu | Interactive Developer Portfolio</title>
</svelte:head>

<a class="skip-link" href="#main">Skip to content</a>

<CommandPalette bind:open={paletteOpen} {projects} onClose={() => (paletteOpen = false)} />

<header class="site-header">
  <a class="brand" href="#hero" aria-label="Go to homepage">
    <span>CJ</span>
    <strong>Chizaram</strong>
  </a>
  <nav aria-label="Primary navigation">
    <a href="#projects">Projects</a>
    <a href="#skills">Skills</a>
    <a href="#contact">Contact</a>
  </nav>
  <div class="header-actions">
    <button class="icon-button" type="button" aria-label="Open command palette" on:click={() => (paletteOpen = true)}>
      Ctrl K
    </button>
    <button class="icon-button" type="button" aria-label="Toggle theme" on:click={toggleTheme}>
      <span aria-hidden="true">{theme === 'dark' ? '☀' : '☾'}</span>
    </button>
  </div>
</header>

<main id="main">
  <section id="hero" class="hero section-panel">
    <ParticleField />
    <div class="hero-grid">
      <div class="hero-copy reveal">
        <p class="eyebrow hero-title">Frontend Developer / Svelte Experience Builder</p>
        <h1>
          <span>Miss Julius</span>
          <span>Chizaram</span>
          <span>Onyinyechukwu</span>
        </h1>
        <p class="hero-intro">
          I build polished product interfaces where motion, clarity, performance, and accessibility work together.
          This portfolio is designed as a live cockpit for my projects, not a static profile.
        </p>
        <div class="hero-actions">
          <a class="primary-action" href="#projects">Explore projects</a>
          <a class="secondary-action" href="/Julius_Chizaram_Resume.pdf" download>Resume PDF</a>
          <a class="secondary-action" href="/Julius_Chizaram_Resume.docx" download>Resume Word</a>
          <a class="secondary-action" href="https://www.linkedin.com/in/chizaram-julius-608714181" target="_blank" rel="noreferrer">LinkedIn</a>
          <a class="secondary-action" href="https://github.com/chizaram-julius" target="_blank" rel="noreferrer">GitHub</a>
        </div>
      </div>

      <aside class="developer-console reveal" aria-label="Interactive developer console">
        <div class="window-bar">
          <span></span><span></span><span></span>
          <strong>portfolio.engine</strong>
        </div>
        <pre><code>$ boot --candidate="Chizaram"
status: immersive portfolio online
routes: hero / projects / skills / contact
motion: prefers-reduced-motion aware
security: input sanitized
shortcut: Ctrl + K command palette</code></pre>
      </aside>
    </div>
  </section>

  <section class="metrics" aria-label="Portfolio engineering highlights">
    <div><strong>4</strong><span>shipped projects</span></div>
    <div><strong>100%</strong><span>responsive intent</span></div>
    <div><strong>SSR</strong><span>SvelteKit static output</span></div>
    <div><strong>A11y</strong><span>keyboard first</span></div>
  </section>

  <section id="projects" class="projects section-panel">
    <div class="section-heading">
      <p class="eyebrow">Project transmission</p>
      <h2>Work that proves range</h2>
      <p>
        Filter the project signal, inspect the engineering story, then jump into the live app, repository, or demo video.
      </p>
    </div>

    <div class="tabs" role="tablist" aria-label="Project categories">
      {#each categories as category}
        <button
          type="button"
          class:active={activeCategory === category}
          aria-pressed={activeCategory === category}
          on:click={() => (activeCategory = category)}
        >
          {category}
        </button>
      {/each}
    </div>

    <div class="project-grid">
      {#each visibleProjects as project, index (project.id)}
        <ProjectCard {project} active={index === 0} />
      {/each}
    </div>
  </section>

  <section id="skills" class="skills section-panel">
    <div class="section-heading">
      <p class="eyebrow">Capability map</p>
      <h2>Built like a frontend system</h2>
    </div>
    <div class="skill-board">
      {#each skills as skill, index}
        <span style={`--delay:${index * 45}ms`}>{skill}</span>
      {/each}
    </div>
    <div class="story-grid">
      <article>
        <h3>Animation decisions</h3>
        <p>Canvas motion stays behind content, reveal effects are transform/opacity based, and reduced-motion users keep the atmosphere without constant movement.</p>
      </article>
      <article>
        <h3>Performance posture</h3>
        <p>Native CSS, route-level SvelteKit output, lazy external links, no heavy animation library, and static deployment keep the experience quick.</p>
      </article>
      <article>
        <h3>Creative layer</h3>
        <p>The command palette and developer console turn the portfolio into a navigable tool, giving evaluators something memorable to interact with.</p>
      </article>
    </div>
  </section>

  <section id="contact" class="contact section-panel">
    <div class="section-heading">
      <p class="eyebrow">Open channel</p>
      <h2>Let's build something sharp</h2>
      <p>Send a quick note. The form validates your contact details, sanitizes input, and opens a Gmail draft directly.</p>
    </div>

    <form on:submit|preventDefault={submitContact} novalidate>
      <label>
        Name
        <input bind:value={form.name} name="name" autocomplete="name" class:error={errors.name} />
        {#if errors.name}<small>{errors.name}</small>{/if}
      </label>
      <label>
        Email
        <input bind:value={form.email} name="email" type="email" autocomplete="email" class:error={errors.email} />
        {#if errors.email}<small>{errors.email}</small>{/if}
      </label>
      <label>
        Message <span class="optional">(optional)</span>
        <textarea bind:value={form.message} name="message" rows="5" class:error={errors.message}></textarea>
        {#if errors.message}<small>{errors.message}</small>{/if}
      </label>
      <button class="primary-action" type="submit">{sent ? 'Gmail draft opened' : 'Open Gmail draft'}</button>
    </form>
  </section>
</main>

<footer>
  <span>Miss Julius Chizaram Onyinyechukwu</span>
  <a href="https://www.linkedin.com/in/chizaram-julius-608714181" target="_blank" rel="noreferrer">LinkedIn</a>
  <a href="https://github.com/chizaram-julius" target="_blank" rel="noreferrer">GitHub</a>
</footer>
