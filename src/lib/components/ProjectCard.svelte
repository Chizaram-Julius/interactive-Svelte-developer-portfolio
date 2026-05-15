<script>
  export let project;
  export let active = false;
</script>

<article id={project.id} class:active style={`--accent:${project.accent}`}>
  <div class="card-visual" aria-hidden="true">
    <span>{project.category}</span>
    <strong>{project.title.slice(0, 2).toUpperCase()}</strong>
  </div>
  <div class="card-copy">
    <p class="eyebrow">{project.year} / {project.category}</p>
    <h3>{project.title}</h3>
    <p>{project.description}</p>
    <div class="tech-list" aria-label={`${project.title} technologies`}>
      {#each project.technologies as tech}
        <span>{tech}</span>
      {/each}
    </div>
    <div class="card-actions">
      <a href={project.live} target="_blank" rel="noreferrer">Live demo</a>
      <a href={project.github} target="_blank" rel="noreferrer">GitHub</a>
      <a href={project.video} target="_blank" rel="noreferrer">Video</a>
    </div>
  </div>
</article>
