# Changelog

## 2026-11-04 (evening)

- Restored the configurable build. Commit f01025e had replaced the generic assets with the dandoy.me site copy, which dropped `data-*` and `window.AgenticBuildIntro` config and the `--lx-*` tokens. All of that is back, with every later fix applied on top.
- Header first: the page holds at the top until the header and nav are fully built. New `holdUntil` option.
- Image wipe is 1.5x faster (1133 ms instead of 1700 ms), scan overlay scaled to match.
- Speed ramp by page position, 1.0x at the top, `rampMid` (1.2x) at `rampFrom`, `rampEnd` (1.5x) at the footer. One shared clock for cursors, boxes, typing, wipes, scan and scroll. Pacing uses the scaled clock so it does not fight the ramp.
- Word-wrap-stable typing: the text is laid out in full as hidden word spans and revealed word by word. Fixes the line jumping and small scroll stutter on long paragraphs.
- Start screen on reload: the session flag only skips the intro on in-site navigation or back and forward, not on reload or a typed URL. Updated head snippet.
- Replay pill: flat periwinkle `#a9b1f2` button labeled "Replay animation", placed in normal flow above the footer in a reserved slot, safe-area aware, replays the full build from a clean state. Options `replay`, `replayLabel`, `replayBefore`, tokens `--lx-replay-*`.

## 2026-11-04 (f01025e)

- Sync fix: real-time pacing with no speed multiplier, strict reading order, calm follow scroll, wrapper elements removed from the queue, 40 s hard stop.

## Earlier

- 7315ed9 build every element to the footer with adaptive pacing.
- e36b276 smoother pacing and calmer follow scroll.
- 600974c follow the active build at the upper third of the viewport.
- 26eeed2 initial skill.
