# Tech Guides — Project Instructions for Claude

This repo is a static HTML blog (GitHub Pages) at https://captainarslan.github.io/tech-guides/.
Every page is a single self-contained `.html` file — no build step, no framework.

## File structure

```
index.html                                        ← blog homepage (guide index)
deploy-laravel-vps/
  index.html                                      ← Laravel VPS full guide
python-systemd-guide/
  index.html                                      ← Python systemd guide Part 1
  production-deployment.html                      ← Python systemd guide Part 2
```

## Design system — apply to every page

### Fonts

Always load both faces from Google Fonts in `<head>`:

```html
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="stylesheet" href="https://fonts.googleapis.com/css2?family=IBM+Plex+Mono:wght@400;500;600&family=Space+Grotesk:wght@400;500;600;700&display=swap">
```

| Role | Face | Notes |
|---|---|---|
| Body / prose | `'Space Grotesk', 'Segoe UI', system-ui, sans-serif` | `font-size: 15px; line-height: 1.65` |
| Headings (h1–h3) | Space Grotesk 700 | `letter-spacing: -0.02em` |
| Code / terminal / mono labels | `'IBM Plex Mono', monospace` | terminal bodies, copy-btn text, eyebrows, tags, metadata |

Never swap the two faces. Space Grotesk is for reading; IBM Plex Mono is for code and UI chrome.

### Color tokens — dark-first

Every new page **must** open its `<style>` with these exact tokens and values:

```css
/* guide pages use the alias names below; homepage uses --bg, --fg, --accent etc. */
/* For guide pages, keep the alias names (--amber, --panel, --text, --line, …) */

:root {                          /* DARK (default) */
  --bg:        #0a0a0b;
  --panel:     #111114;
  --panel-2:   #1b1b1f;
  --panel-3:   #252529;
  --line:      #26262a;
  --line-2:    #38383d;
  --amber:     #d97706;          /* primary accent */
  --amber-dim: #b45309;
  --amber-soft:#78350f;
  --text:      #e3dfd7;
  --text-dim:  #9a9691;
  --text-3:    #66625d;
  --green:     #4ade80;
  --blue:      #60a5fa;
  --red:       #f87171;
  color-scheme: dark;
}

:root[data-theme="light"] {      /* LIGHT */
  --bg:        #f8f7f4;
  --panel:     #efece6;
  --panel-2:   #e5e1da;
  --panel-3:   #d9d5cc;
  --line:      #dbd7d0;
  --line-2:    #ccc8c0;
  --amber:     #b45309;
  --amber-dim: #d97706;
  --amber-soft:#fef3c7;
  --text:      #1a1815;
  --text-dim:  #6b6560;
  --text-3:    #9b9590;
  --green:     #15803d;
  --blue:      #2563eb;
  --red:       #dc2626;
  color-scheme: light;
}
```

The homepage (`index.html`) uses `--fg` / `--fg-2` / `--fg-3` / `--accent` / `--border` / `--border-2`
instead of the guide aliases — those map to the same values. Keep whichever set the file already uses;
never mix the two alias families in one file.

When using hardcoded `rgba(...)` for amber tints, always use `rgba(217,119,6,α)` (dark) or
`rgba(180,83,9,α)` (light) — never `rgba(255,176,0,α)` (old amber).

### Body scanline (dark pages only)

All guide pages include this subtle scanline overlay in dark mode:

```css
body::before {
  content: "";
  position: fixed;
  inset: 0;
  pointer-events: none;
  background: repeating-linear-gradient(
    0deg,
    rgba(217,119,6,0.015) 0px,
    rgba(217,119,6,0.015) 1px,
    transparent 1px,
    transparent 3px
  );
  z-index: 999;
  mix-blend-mode: overlay;
}
:root[data-theme="light"] body::before { display: none; }
```

### Hero gradient

All pages use an amber radial glow behind the hero section:
- Dark: `radial-gradient(ellipse 900px 400px at 50% -10%, rgba(217,119,6,0.08), transparent 70%)`
- Light: `radial-gradient(ellipse 900px 400px at 50% -10%, rgba(180,83,9,0.06), transparent 70%)`

## Shared nav — identical on every page

All pages (homepage and guides) must include this sticky nav. The logo always links to
`index.html` (homepage) — use `../index.html` from guide subfolders.

```html
<nav class="site-nav" role="banner">
  <div class="site-nav-inner">
    <a href="../index.html" class="site-nav-logo" aria-label="All Guides">
      <div class="site-nav-icon" aria-hidden="true">CA</div>
      <span class="site-nav-text"><span>~/</span><span class="site-nav-accent">guides</span></span>
    </a>
    <div class="site-nav-right">
      <a href="https://github.com/CaptainArslan" class="site-nav-link"
         target="_blank" rel="noopener noreferrer">github</a>
      <button class="theme-toggle" id="themeToggle" aria-label="Toggle light/dark mode">
        <svg class="th-moon" width="15" height="15" fill="none" stroke="currentColor"
             stroke-width="1.8" viewBox="0 0 24 24" aria-hidden="true">
          <path d="M21 12.79A9 9 0 1 1 11.21 3 7 7 0 0 0 21 12.79z"/>
        </svg>
        <svg class="th-sun" width="15" height="15" fill="none" stroke="currentColor"
             stroke-width="1.8" viewBox="0 0 24 24" aria-hidden="true">
          <circle cx="12" cy="12" r="5"/>
          <line x1="12" y1="1" x2="12" y2="3"/><line x1="12" y1="21" x2="12" y2="23"/>
          <line x1="4.22" y1="4.22" x2="5.64" y2="5.64"/>
          <line x1="18.36" y1="18.36" x2="19.78" y2="19.78"/>
          <line x1="1" y1="12" x2="3" y2="12"/><line x1="21" y1="12" x2="23" y2="12"/>
          <line x1="4.22" y1="19.78" x2="5.64" y2="18.36"/>
          <line x1="18.36" y1="5.64" x2="19.78" y2="4.22"/>
        </svg>
      </button>
    </div>
  </div>
</nav>
```

Nav CSS (uses guide alias variables; substitute `--accent`/`--fg`/`--border` equivalents on the homepage):

```css
.site-nav {
  position: sticky; top: 0; z-index: 200;
  background: var(--bg); border-bottom: 1px solid var(--line); padding: 13px 24px;
}
.site-nav-inner {
  max-width: 960px; margin: 0 auto;
  display: flex; align-items: center; justify-content: space-between; gap: 16px;
}
.site-nav-logo { display: flex; align-items: center; gap: 10px; text-decoration: none; flex-shrink: 0; }
.site-nav-icon {
  width: 32px; height: 32px; background: var(--amber); border-radius: 8px;
  display: flex; align-items: center; justify-content: center;
  font-family: 'IBM Plex Mono', monospace; font-weight: 600; font-size: 13px;
  color: #fff; letter-spacing: -0.03em;
}
.site-nav-text { font-family: 'IBM Plex Mono', monospace; font-size: 13px; font-weight: 600; color: var(--text-dim); }
.site-nav-accent { color: var(--amber); }
.site-nav-right { display: flex; align-items: center; gap: 8px; }
.site-nav-link {
  font-family: 'IBM Plex Mono', monospace; font-size: 12px; color: var(--text-dim);
  text-decoration: none; padding: 6px 12px; border-radius: 6px; border: 1px solid transparent;
  transition: color .15s, background .15s, border-color .15s;
}
.site-nav-link:hover { color: var(--text); background: var(--panel-2); border-color: var(--line); }
.theme-toggle {
  position: static !important; top: auto !important; right: auto !important;
  border-radius: 8px !important; padding: 8px !important;
  width: 36px; height: 36px; justify-content: center;
  background: var(--panel); border: 1px solid var(--line); cursor: pointer;
  display: flex; align-items: center; color: var(--text-dim);
  transition: color .15s, border-color .15s;
}
.theme-toggle:hover { color: var(--amber); border-color: var(--amber); }
.th-moon { display: block; }
.th-sun  { display: none;  }
:root[data-theme="light"] .th-moon { display: none;  }
:root[data-theme="light"] .th-sun  { display: block; }
```

## Shared footer — identical on every page

```html
<footer class="site-footer">
  <div class="site-footer-inner">
    <p class="site-footer-copy">
      Built by <a href="https://github.com/CaptainArslan"
                  target="_blank" rel="noopener noreferrer">CaptainArslan</a>.
      Free to use, copy &amp; adapt.
    </p>
    <a href="../index.html" class="site-footer-back">
      <svg width="13" height="13" fill="none" stroke="currentColor" stroke-width="2"
           viewBox="0 0 24 24" aria-hidden="true">
        <line x1="19" y1="12" x2="5" y2="12"/>
        <polyline points="12 19 5 12 12 5"/>
      </svg>
      All Guides
    </a>
  </div>
</footer>
```

Footer CSS:

```css
.site-footer { border-top: 1px solid var(--line); padding: 28px 24px; margin-top: 32px; }
.site-footer-inner {
  max-width: 960px; margin: 0 auto;
  display: flex; align-items: center; justify-content: space-between; flex-wrap: wrap; gap: 12px;
}
.site-footer-copy { font-family: 'IBM Plex Mono', monospace; font-size: 12px; color: var(--text-dim); }
.site-footer-copy a { color: var(--amber); text-decoration: none; }
.site-footer-copy a:hover { text-decoration: underline; }
.site-footer-back {
  font-family: 'IBM Plex Mono', monospace; font-size: 12px; color: var(--text-dim);
  text-decoration: none; display: flex; align-items: center; gap: 5px; transition: color .15s;
}
.site-footer-back:hover { color: var(--amber); }
```

## Theme toggle JavaScript

Use this exact snippet in every page's `<script>` block. The `localStorage` key is `"theme"` everywhere:

```js
(function () {
  var root   = document.documentElement;
  var toggle = document.getElementById('themeToggle');

  function apply(theme) {
    root.setAttribute('data-theme', theme);
  }

  var saved = null;
  try { saved = localStorage.getItem('theme'); } catch (e) {}
  var prefersLight = window.matchMedia && window.matchMedia('(prefers-color-scheme: light)').matches;
  apply(saved || (prefersLight ? 'light' : 'dark'));

  toggle.addEventListener('click', function () {
    var next = root.getAttribute('data-theme') === 'light' ? 'dark' : 'light';
    apply(next);
    try { localStorage.setItem('theme', next); } catch (e) {}
  });
})();
```

## Terminal / code block aesthetic

All code blocks (`.term`, `.cb`, `.code-block`, etc.) use:
- Background: `var(--panel)` with border `1px solid var(--line)`
- Top bar: three dots (red `#ff5f57`, yellow `#febc2e`, green `#28c840`) + centred filename label in `var(--text-3)`
- Copy button: IBM Plex Mono 11px, uses `navigator.clipboard.writeText()`

## What NOT to change without updating all pages

If you change any of the following in one page, apply the same change to all pages:

- CSS token values in `:root` / `:root[data-theme="light"]`
- Nav HTML or nav CSS
- Footer HTML or footer CSS
- Theme toggle JavaScript (including the `localStorage` key `"theme"`)
- Font faces or sizes on `body`

## Git workflow

The combined repo lives at `D:\Tech-Blogs` (remote: `github.com/CaptainArslan/tech-guides`).
After making changes:

```bash
cd "D:\Tech-Blogs"
git add .
git commit -m "your message

Co-Authored-By: Claude Sonnet 4.6 <noreply@anthropic.com>"
git push
```

GitHub Pages serves from `master` branch root at https://captainarslan.github.io/tech-guides/.
Build takes ~1–2 minutes after each push.
