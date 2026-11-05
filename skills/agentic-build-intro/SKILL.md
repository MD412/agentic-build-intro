---
name: agentic-build-intro
description: Adds a first-visit intro to a static or plain-HTML site. A start screen shows the owner's name, title and location; on start, a scan traces the real page layout and labeled cursor agents draw boxes, type text and reveal images to build the actual page, which ends flat and normal. Use when asked to add an "agentic build" intro, start screen or build animation to a website.
license: MIT
metadata:
  author: Marc Dandoy
  publisher: MacroGrace Systems
  homepage: https://macrogracesystems.com
  version: "1.0.0"
---

# Agentic Build Intro

A start screen, a scan of the real page, then a build of the real page by labeled cursor agents. No framework, no build step. Two files: `assets/layers.js` and `assets/layers.css`.

## Files

- `assets/layers.js`: the runtime. Reads config, shows the gate, runs the scan and the build.
- `assets/layers.css`: all styles, themed through `--lx-*` custom properties.
- `assets/head-snippet.html`: the three tags every page needs in its head.
- `references/gate-design.html`, `references/scan-design.html`: the original standalone designs. Use them as the starting point if the owner wants a new gate or scan design.

## Steps

1. **Back up the site.** Then copy `layers.js` and `layers.css` into it (default paths `/js/layers.js` and `/css/layers.css`; adjust the snippet if the site uses others).
2. **Add the head tags to every page.** Paste `assets/head-snippet.html` into each page's `<head>`. The inline script hides the page before paint only when the intro will run, and unhides it after 4s if the runtime never loads.
3. **Set the gate copy.** Use `data-*` attributes on the script tag or a `window.AgenticBuildIntro` object set before the script loads. The object wins over attributes.

   | Key | Attribute | Default |
   |---|---|---|
   | `name` | `data-name` | `document.title` |
   | `initials` | `data-initials` | first letters of `name` |
   | `title` | `data-title` | hidden when empty |
   | `location` | `data-location` | hidden when empty |
   | `timezone` | `data-timezone` | visitor's local time |
   | `clock` | `data-clock` | `true` |
   | `cta` | `data-cta` | `Initiate Portfolio Build` |
   | `skip` | `data-skip` | `Skip to site` |
   | `agents` | `data-agents` | 30 desktop, 14 phone |
   | `colors` | `data-colors` | built-in palette (comma list) |

   Use only facts the owner gave you. Never invent a name, title or location. Any extra copy is neutral UI text.
4. **Match the site's design system.** Read the site's CSS variables and fonts, then override the `--lx-*` tokens on `:root` in the site's own stylesheet: `--lx-bg`, `--lx-surface`, `--lx-stroke`, `--lx-text`, `--lx-muted`, `--lx-soft`, `--lx-ink`, `--lx-accent-1` to `--lx-accent-4`, `--lx-font-display`, `--lx-font-body`. The kit does not load fonts. Keep the site's own colors and pick vibrant agent colors that read on its background.
5. **Do not change how the scan works.** The scan outlines come from the same element list the build uses, so outlines sit exactly where the real content lands and the scan agents hand off to the build agents at the same spots, colors and labels. Never swap in a generic wireframe.
6. **Keep these behaviors.**
   - Boxes drag out with handles and W x H labels, text types with a caret, guide lines flash while agents work.
   - Images reveal over about 1.7s with a stepped top-down wipe, scan line, faint stripes and a grayscale to color settle.
   - Follow is on by default: as each element starts building, the page scrolls smoothly so the active build sits about 30% from the top of the viewport, following the agents down. It never scrolls back to the top at the end. With reduced motion it jumps instead of easing.
   - Runs once per session. `?build=1` replays, `?build=0` skips. Enter starts, Esc skips the gate; wheel, click, touch or any key skips a running intro.
   - Works down to 390px wide with fewer agents on phones. `prefers-reduced-motion` skips the intro.
7. **Code rules.** No code comments, no em dashes in copy, no AI-generated human figures.
8. **Verify by recording.** Run `node --check` on the JS. Serve the site and record Playwright video at 1280x800 and 390x844: open `?build=1`, click the start button, record 20 to 60s. Pull frames for the gate, scan, handoff, build and end state, check for page errors, and fix what you see. Confirm every existing page still returns 200.
9. **Report** with the live link, a gate screenshot and the recording.
