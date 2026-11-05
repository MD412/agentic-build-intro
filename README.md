# agentic-build-intro

An Agent Skill that adds a first-visit intro to any static or plain-HTML site. A start screen shows your name, title and location. Press start and a scan traces the real page layout, then labeled cursor agents build the actual page: they drag out boxes with size labels, type the text with a caret and reveal images. The page ends flat and normal.

Plain JS and CSS. No framework, no build step, about 64 KB of JS and CSS, unminified.

Live example: https://dandoy-me.vercel.app/?build=1

Repo: https://github.com/MD412/agentic-build-intro

Now part of a family of intros: https://github.com/MD412/macrograce-intros

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
<script>(function(){var r=document.documentElement,q=location.search;function inSite(){try{var n=performance.getEntriesByType('navigation')[0];if(n&&n.type==='reload')return 0;if(n&&n.type==='back_forward')return 1;var f=document.referrer;return f&&new URL(f).origin===location.origin?1:0}catch(e){return 0}}try{if(/[?&]build=0/.test(q)||matchMedia('(prefers-reduced-motion: reduce)').matches||(!/[?&]build=1/.test(q)&&sessionStorage.getItem('lx-built')&&inSite()))return}catch(e){}r.classList.add('lx-pre');setTimeout(function(){if(!document.getElementById('lx-gate'))r.classList.remove('lx-pre')},4000)})()</script>
<link rel="stylesheet" href="/css/layers.css">
<script src="/js/layers.js" defer data-name="Your Name" data-title="Your Title" data-location="City, ST" data-timezone="America/New_York"></script>
```

The inline script hides the page before first paint so visitors never see a flash of the finished page. If `layers.js` fails to load, the page unhides after 4 seconds.

## URL params

| Param | Effect |
|---|---|
| `?build=1` | Show the intro again, even if it already ran this session |
| `?build=0` | Skip the intro |

The intro shows on every fresh visit and every reload. It is skipped only when the visitor arrives from another page of the same site, or goes back or forward, after it already ran in that tab. Enter starts it, Esc skips the start screen, and wheel, click, touch or any key skips a running build. Visitors with `prefers-reduced-motion` never see it.

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
| `holdUntil` | `data-hold-until` | auto: bottom of the top `header`, `nav` or `[role=banner]` |
| `rampFrom` | `data-ramp-from` | auto: 50% of the page height |
| `rampMid` | `data-ramp-mid` | `1.2` |
| `rampEnd` | `data-ramp-end` | `1.5` (set `rampMid` and `rampEnd` to `1` to disable) |
| `replay` | `data-replay` | `true` (set `false` to hide the replay pill) |
| `replayLabel` | `data-replay-label` | `Replay animation` |
| `replayBefore` | `data-replay-before` | last `footer` or `[role=contentinfo]`, else end of body |

## How the build behaves

- Header first. The page does not scroll until every element above `holdUntil` (by default the top header and nav) is fully built, including typing and image wipes.
- Speed ramp. Normal speed at the top, then a smooth increase by page position: `rampMid` at `rampFrom`, easing up to `rampEnd` at the footer. Cursors, boxes, typing, image wipes, the scan overlay and the follow scroll all use the same clock, so they stay in sync.
- Stable typing. Text is laid out in full from the start as hidden word spans and revealed word by word, so line wrapping never changes mid-type.
- Replay. When the build finishes, a pill button fades in, in normal page flow just above the footer, so it never covers content. It smoothly scrolls to the top and replays the full build from a clean state.

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
  --lx-replay-bg: #a9b1f2;
  --lx-replay-hover: #bcc2f6;
  --lx-replay-active: #979fe8;
  --lx-replay-stroke: #c3c9f7;
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
  CHANGELOG.md
  references/gate-design.html
  references/scan-design.html
```

## License

MIT. Built by Marc Dandoy at [MacroGrace Systems](https://macrogracesystems.com).
