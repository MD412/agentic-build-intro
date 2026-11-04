# agentic-build-intro

An Agent Skill that adds a first-visit intro to any static or plain-HTML site. A start screen shows your name, title and location. Press start and a scan traces the real page layout, then labeled cursor agents build the actual page: they drag out boxes with size labels, type the text with a caret and reveal images. The page ends flat and normal.

Plain JS and CSS. No framework, no build step, about 64 KB of JS and CSS, unminified.

Live example: https://dandoy-me.vercel.app/?build=1

Repo: https://github.com/MD412/agentic-build-intro

## Install for agents

```bash
npx skills add MD412/agentic-build-intro
```

Claude Code plugin marketplace:

```
/plugin marketplace add MD412/agentic-build-intro
/plugin install agentic-build-intro@macrogracesystems
```

Then ask your agent to "add the agentic build intro to this site".

## Manual install

Copy `skills/agentic-build-intro/assets/layers.js` and `layers.css` into your site, then add these three tags to the `<head>` of every page:

```html
<script>(function(){var r=document.documentElement,q=location.search;try{if(/[?&]build=0/.test(q)||matchMedia('(prefers-reduced-motion: reduce)').matches||(!/[?&]build=1/.test(q)&&sessionStorage.getItem('lx-built')))return}catch(e){}r.classList.add('lx-pre');setTimeout(function(){if(!document.getElementById('lx-gate'))r.classList.remove('lx-pre')},4000)})()</script>
<link rel="stylesheet" href="/css/layers.css">
<script src="/js/layers.js" defer data-name="Your Name" data-title="Your Title" data-location="City, ST" data-timezone="America/New_York"></script>
```

The inline script hides the page before first paint so visitors never see a flash of the finished page. If `layers.js` fails to load, the page unhides after 4 seconds.

## URL params

| Param | Effect |
|---|---|
| `?build=1` | Show the intro again, even if it already ran this session |
| `?build=0` | Skip the intro |

The intro runs once per browser session. Enter starts it, Esc skips the start screen, and wheel, click, touch or any key skips a running build. Visitors with `prefers-reduced-motion` never see it.

## Customize the copy

Use `data-*` attributes on the script tag, or set a config object before the script loads. The object wins.

```html
<script>
  window.AgenticBuildIntro = {
    name: 'Ada Lovelace',
    title: 'Analytical Engine Programmer',
    location: 'London, UK',
    timezone: 'Europe/London',
    cta: 'Start the build',
    skip: 'Skip to site',
    agents: 24,
    colors: ['#ff5fa2', '#ffb347', '#7ee0c3', '#79c8ff']
  };
</script>
```

| Key | Attribute | Default |
|---|---|---|
| `name` | `data-name` | `document.title` |
| `initials` | `data-initials` | first letters of `name` |
| `title` | `data-title` | hidden when empty |
| `location` | `data-location` | hidden when empty |
| `timezone` | `data-timezone` | visitor's local time |
| `clock` | `data-clock` | `true` (set `false` to hide) |
| `cta` | `data-cta` | `Initiate Portfolio Build` |
| `skip` | `data-skip` | `Skip to site` |
| `agents` | `data-agents` | 30 on desktop, 14 on phones |
| `colors` | `data-colors` | built-in palette, comma separated in the attribute |

## Customize the tokens

Override any of these on `:root` in your own stylesheet. The kit's defaults have zero specificity, so yours always win.

```css
:root {
  --lx-bg: #2d2650;
  --lx-surface: rgba(53, 49, 97, 0.38);
  --lx-stroke: #483d7e;
  --lx-text: #f7f8ff;
  --lx-muted: #dcdcea;
  --lx-soft: #cbceea;
  --lx-ink: #1c1735;
  --lx-accent-1: #ff5fa2;
  --lx-accent-2: #ffb347;
  --lx-accent-3: #ffd075;
  --lx-accent-4: #ff28ea;
  --lx-font-display: "Exo", system-ui, sans-serif;
  --lx-font-body: "Space Grotesk", system-ui, sans-serif;
}
```

`--lx-ink` is the text color on accent fills (buttons, labels), so keep it dark against your accents. The kit does not load fonts; load them yourself or let the stack fall back.

## Layout

```
skills/agentic-build-intro/
  SKILL.md
  assets/layers.js
  assets/layers.css
  assets/head-snippet.html
  references/gate-design.html
  references/scan-design.html
```

## License

MIT. Built by Marc Dandoy at [MacroGrace Systems](https://macrogracesystems.com).
