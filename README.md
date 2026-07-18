# Portfolio

A single-page personal portfolio. No build step, no dependencies to install — GitHub Pages serves `index.html` directly.

**Live:** https://danons.github.io

---

## Editing

Everything lives in `index.html`. Three places you'll actually touch:

### 1. Adding a project

Near the bottom of the file, find the `PROJECTS` array. Append one object:

```js
{
  title:    'Project Name',
  blurb:    'One or two sentences about what it is.',
  category: 'code',                          // 'code' | 'design' | 'writing'
  tags:     ['React', 'Node.js'],
  image:    'assets/projects/name.jpg',      // or null for a gradient tile
  accent:   ['#8b5cf6', '#22d3ee'],          // gradient colours (used when image is null)
  links:    { demo: 'https://…', source: 'https://…' }   // omit any you don't have
}
```

Valid `links` keys are `demo`, `source`, and `writeup`. Each renders with its own icon and label.

If `image` is `null` the card shows a gradient tile with the project's initials — useful before you have screenshots.

### 2. Your details

Search `index.html` for these and replace:

| Placeholder | Where |
| --- | --- |
| `Your Name` | `<title>`, nav brand, hero, about, footer |
| `YN` | the `#avatar` initials |
| `HANDLE` | LinkedIn / X links in the contact section (currently commented out) |
| `darulquro17@gmail.com` | contact button (appears twice: `href` and label) |

For a real photo: drop a square image at `assets/avatar.jpg`, then replace
`<div class="avatar" id="avatar">YN</div>` with
`<div class="avatar"><img src="assets/avatar.jpg" alt="Your Name"></div>`.

### 3. Colours

All theming runs through CSS custom properties at the top of the `<style>` block — `:root` for dark, `[data-theme="light"]` for light. Change `--accent`, `--accent-2`, `--accent-3` and the whole site follows.

The WebGL hero has its own copy of the palette in the `PALETTES` object (RGB as 0–1 floats, since that's what shaders take). Update both if you want them to match.

---

## Running locally

ES modules don't work over `file://`, so serve it:

```bash
python -m http.server 8000
# or: npx serve
```

Then open http://localhost:8000

---

## Deploying

The repo is named `Danons.github.io`, so anything pushed to `main` publishes to the root URL within a minute or two:

```bash
git add .
git commit -m "Update projects"
git push
```

---

## How it's built

- **Three.js** — the hero background is one fullscreen plane running a domain-warped fbm noise shader. Single draw call.
- **GSAP + ScrollTrigger** — scroll-triggered reveals.
- **Lenis** — smooth inertial scrolling.

All three load from CDNs. Each one is optional at runtime: if a CDN is blocked, the page falls back to a CSS gradient hero, instant reveals, and native scrolling. It also renders fully with JavaScript disabled.

The shader is skipped entirely under 640px wide and for anyone with `prefers-reduced-motion: reduce`, and pauses when the hero scrolls out of view or the tab is hidden.

`.nojekyll` stops GitHub Pages running its Jekyll build, which we don't need.
