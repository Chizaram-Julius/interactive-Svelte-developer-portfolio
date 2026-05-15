<script>
  export let open = false;
  export let projects = [];
  export let onClose = () => {};

  const actions = [
    { label: 'Review featured projects', target: '#projects' },
    { label: 'Inspect capability map', target: '#skills' },
    { label: 'Contact Chizaram', target: '#contact' },
    { label: 'Download resume PDF', target: '/Julius_Chizaram_Resume.pdf', download: true },
    { label: 'Download editable Word resume', target: '/Julius_Chizaram_Resume.docx', download: true }
  ];

  function go(action) {
    if (action.download) {
      window.location.href = action.target;
    } else if (action.external) {
      window.open(action.target, '_blank', 'noopener,noreferrer');
    } else {
      document.querySelector(action.target)?.scrollIntoView({ behavior: 'smooth' });
    }
    onClose();
  }
</script>

{#if open}
  <div class="palette-backdrop" role="presentation">
    <button class="palette-hit" type="button" aria-label="Close command palette" on:click={onClose}></button>
    <div class="palette" role="dialog" aria-modal="true" aria-labelledby="palette-title">
      <div>
        <p class="eyebrow">Command center</p>
        <h2 id="palette-title">Portfolio navigation</h2>
      </div>
      <div class="command-list">
        {#each actions as action}
          <button type="button" on:click={() => go(action)}>
            <span>{action.label}</span>
            <kbd>Open</kbd>
          </button>
        {/each}
      </div>
      <div class="project-jumps" aria-label="Project shortcuts">
        {#each projects as project}
          <a href={`#${project.id}`} on:click={onClose}>{project.title}</a>
        {/each}
      </div>
    </div>
  </div>
{/if}
