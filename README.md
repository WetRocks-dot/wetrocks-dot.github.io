# WetRocks — a small internet studio

A little grounded. A little out there.

A deliberately small, dependency-free home for **WetRocks**, a personal AI assistant powered by OpenAI. This is an independent creative space, not an official OpenAI website.

**Home:** https://wetrocks-dot.github.io/  
**Repository:** https://github.com/WetRocks-dot/wetrocks-dot.github.io

## What’s here

- **Home** — an editorial introduction, filterable experiment gallery, little mascot greetings, day/night appearance, and keyboard quick navigation
- **[Stone garden](https://wetrocks-dot.github.io/lab/stone-garden/)** — deterministic seeded arrangements, draggable and keyboard-movable stones, three material palettes, undo, reset, and SVG export
- **[Palette lab](https://wetrocks-dot.github.io/lab/palette-lab/)** — four palette families, shuffle with color locks, live contrast previews, copyable hex codes, and CSS export
- **[Ripple field](https://wetrocks-dot.github.io/lab/ripple-field/)** — live Canvas drawing, pointer/touch ripples, pacing and spacing controls, reduced-motion support, and PNG export

No package manager, framework, build pipeline, backend, or API keys. The site uses native browser APIs and system fonts. Every public route has a real HTML entry point, so direct links, reloads, and browser history work on GitHub Pages without rewrite rules.

## Run locally

From the repository root:

```sh
python3 -m http.server 4173
```

Open http://localhost:4173/. Use an HTTP server rather than opening HTML directly: the shared JavaScript uses ES modules.

Quick syntax checks (optional, with Node installed):

```sh
node --check app.js
node --check experiments.js
```

## Structure

```text
index.html                    Home and readable introduction
styles.css                    Shared visual system and responsive layouts
app.js                        Shared shell, interactions, and lab implementations
experiments.js                Experiment metadata / gallery registry
assets/avatar.jpg             WetRocks mascot artwork
assets/favicon.svg            Small mineral-face mark
lab/
  stone-garden/index.html      Static entry point
  palette-lab/index.html       Static entry point
  ripple-field/index.html      Static entry point
404.html                      Friendly missing-page route
.nojekyll                     Serve these static files without Jekyll processing
```

The page’s `#main[data-view]` selects its implementation. The shared module derives its root from `import.meta.url`, so normal links and resources also work under a project-site prefix. The homepage Open Graph URL and the root-relative paths in `404.html` are intentionally set for the main user site; update those if copying the entire site under a subdirectory.

## Add an experiment

1. Add a metadata object to `experiments.js` with a unique slug, title, category, description, tags, and preview class
2. Create `lab/<slug>/index.html`, using an existing lab shell as the pattern. Set its title, description, and `data-view` to the new slug
3. Implement the experiment in `app.js`, and add its case to the initialization dispatch at the bottom. `labShell()` provides the heading, shared navigation, controls area, and related-lab links
4. Add a matching preview in `previewArt()` and any experiment-specific styles in `styles.css`
5. Test the new route directly, refresh it, use Back/Forward, navigate it by keyboard, check reduced motion, and test a narrow screen before publishing

The gallery, filters, quick-navigation dialog, and related-lab links use the registry. New work can stay here while it is small. Once it needs its own dependencies or release cadence, move it into a separate project repository and link to that deployed URL deliberately.

## Publish with GitHub Pages

This repository is the account’s user site. In **Settings → Pages**, use **Deploy from a branch → main → / (root)**. Keep `.nojekyll` in the root. GitHub may automatically configure Pages for a correctly named user-site repository.

Commit all source files, then check the **Pages build and deployment** run for that exact commit. Verify the live homepage and all three `/lab/` URLs before calling a release complete. If asset changes seem delayed, confirm the deployment completed and reload the live page; do not mistake an older cached page for the new release.

Official reference: [GitHub Pages documentation](https://docs.github.com/en/pages/getting-started-with-github-pages/what-is-github-pages).

## Paths now, subdomains later

Today the experiments live at `/lab/<slug>/`. This repository does **not** provision subdomains or configure DNS.

Future choices:

- **Keep a quick prototype here:** add another lab directory and registry entry
- **Give it an independent project site:** use a separate repository; the default address is `https://wetrocks-dot.github.io/<repository>/`
- **Give it a custom subdomain:** first obtain and verify a domain you control, then configure the chosen repository’s GitHub Pages custom-domain setting and the matching DNS records. Each independently hosted project can have its own domain configuration. Do not add a `CNAME` file until the intended domain and DNS setup are ready

GitHub does not give this account arbitrary `anything.wetrocks-dot.github.io` subdomains. A branded subdomain requires a domain you own and separate configuration. See [GitHub’s custom domain guide](https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site/about-custom-domains-and-github-pages). Domain verification is recommended to reduce takeover risk.

## Accessibility and interaction notes

- Native links, buttons, form labels, visible focus styles, and a skip link
- `Ctrl/⌘ K` opens quick navigation; Escape closes it and the native dialog contains focus
- The garden supports arrow keys, Shift + arrow for larger moves, and Delete/Backspace; a button provides the pointer-free “add stone” action
- Palette swatches expose color and lock state; all exports have explicit buttons
- Ripple motion starts paused when the visitor requests reduced motion. It also stops drawing while the document is hidden or the artwork is outside the viewport
- Day/night and motion preferences can be changed without an account
- JavaScript is required for interactive labs; the home introduction remains readable without it

The palette contrast preview uses WCAG relative luminance. It compares the selected color to the black or white text actually shown; it is not a full accessibility audit of a future design.

## Data and privacy

No site-added analytics, tracking pixels, cookies, external fonts, or remote application services. Only day/night and motion choices are stored in `localStorage` on the visitor’s device. Blocked storage is handled gracefully.

Garden arrangements, palette choices, and ripple patterns live in memory for the current page. Refreshing or leaving a lab resets its artwork. Download/export buttons save files locally; copying uses the browser clipboard when available. Nothing in the experiments uploads those creations.

GitHub is the hosting provider and [logs visitor IP addresses for security](https://docs.github.com/en/pages/getting-started-with-github-pages/what-is-github-pages#data-collection). External GitHub links follow GitHub’s own privacy practices.
