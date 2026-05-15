# Interactive Developer Portfolio with SvelteKit

An immersive portfolio for Miss Julius Chizaram Onyinyechukwu. It is built as an interactive frontend experience rather than a static profile page.

## Setup

```bash
npm install
npm run dev
```

Build for production:

```bash
npm run build
npm run preview
```

## Live Demo

https://interactive-svelte-developer-portfoli.netlify.app/

## Repository

https://github.com/Chizaram-Julius/interactive-Svelte-developer-portfolio

## Tech Stack

- Framework: SvelteKit
- Language: Svelte, JavaScript, HTML, CSS
- Build Tool: Vite
- Rendering/Deployment Mode: Static generation with `@sveltejs/adapter-static`
- Styling: Native CSS with CSS variables for themes and responsive design
- Animation: Native CSS transitions/keyframes plus a lightweight Canvas particle system
- State/Interaction: Svelte reactive state, component props, local component state, and `localStorage` theme persistence
- Accessibility: Semantic HTML, skip link, visible focus states, keyboard navigation, and reduced-motion support
- Contact Handling: Client-side validated Gmail compose integration
- Hosting Target: Netlify

## Architecture

- `src/routes/+page.svelte` composes the full portfolio route.
- `src/lib/data/projects.js` stores project metadata, demo links, GitHub links, videos, categories, and skills.
- `src/lib/components/ParticleField.svelte` renders the animated canvas background.
- `src/lib/components/CommandPalette.svelte` provides keyboard-driven navigation.
- `src/lib/components/ProjectCard.svelte` renders reusable project showcase cards.
- `src/styles.css` contains the design system, responsive rules, motion rules, and theme tokens.

The project uses SvelteKit with the static adapter so it can deploy cleanly to Netlify, Vercel, Cloudflare Pages, or similar hosts.

## Project Structure

```text
.
|-- static/
|   |-- chizaram-julius-resume.txt
|   |-- Julius_Chizaram_Resume.pdf
|   |-- Julius_Chizaram_Resume.docx
|   |-- resume.html
|   `-- favicon.svg
|-- src/
|   |-- app.html
|   |-- styles.css
|   |-- lib/
|   |   |-- components/
|   |   |   |-- CommandPalette.svelte
|   |   |   |-- ParticleField.svelte
|   |   |   `-- ProjectCard.svelte
|   |   `-- data/
|   |       `-- projects.js
|   `-- routes/
|       |-- +layout.js
|       |-- +layout.svelte
|       `-- +page.svelte
|-- package.json
|-- svelte.config.js
|-- vite.config.js
`-- README.md
```

- `static/` contains public files served directly by the app, including the PDF and editable Word resume downloads.
- `static/chizaram-julius-resume.txt` is the editable source content used to keep the resume copy current.
- `src/app.html` defines the base HTML document and SEO metadata.
- `src/styles.css` holds the global visual system, responsive layout rules, theme tokens, and animation styles.
- `src/lib/components/` contains reusable interactive UI components.
- `src/lib/data/projects.js` centralizes portfolio content so projects can be rendered dynamically.
- `src/routes/+layout.js` enables prerendering for static deployment.
- `src/routes/+page.svelte` assembles the full interactive portfolio experience.

## Animation Decisions

Animations are intentionally transform- and opacity-based where possible. The hero uses a lightweight canvas particle field, project cards use hover elevation, and skills use staggered reveal timing. Users with `prefers-reduced-motion` receive reduced animation automatically.

## Performance Optimization

- Static SvelteKit output.
- Native CSS instead of a large UI framework.
- No heavy animation library.
- Canvas particle count is capped and device-pixel-ratio is limited.
- External project/video links are loaded only when clicked.
- CSS variables support theme switching without re-rendering the whole UI.

## Accessibility

- Semantic landmarks: header, nav, main, sections, footer.
- Skip navigation link.
- Visible focus states.
- Keyboard shortcut for command palette: `Ctrl + K` or `Cmd + K`.
- Accessible labels on navigation and controls.
- Reduced-motion support.
- Readable contrast across dark and light themes.

## Security And Stability

- Contact form input is sanitized before generating the mail link.
- Validation prevents empty or malformed contact submissions.
- External links use `rel="noreferrer"`.
- No secrets are stored in the client.

## Trade-offs

- The contact form opens a Gmail compose draft so users are not blocked by missing desktop mail-client setup.
- The resume CTA offers only the requested PDF and editable Word document; both are generated from the updated in-project resume content.
- Project screenshots are represented by generated visual cards and linked Loom demos to avoid adding heavy media assets to the first load.
