# Sponskip ASCII Homepage Implementation Plan

> **For agentic workers:** Execute these tasks inline in this existing Sponskip website worktree. Do not commit or push; Blake's vault instructions reserve that for his hooks and sync agent.

**Goal:** Give sponskip.com a futuristic ASCII-led hero with a prominent SPONSKIP wordmark and an atmospheric character backdrop.

**Architecture:** Keep the static HTML site. Add one decorative hero canvas, a fixed ASCII wordmark in a `<pre>`, and small scoped JavaScript for the canvas. Preserve the current demo and release/install flows.

**Tech Stack:** HTML, CSS, vanilla JavaScript, Tailwind CDN already on the page.

---

### Task 1: Hero composition and copy

**Files:** Modify `index.html`; keep all existing links and demo behavior.

- [x] Replace the current centered headline block with a full-width hero surface whose content uses a constrained inner column.
- [x] Add a fixed `<pre class="ascii-wordmark" aria-hidden="true">` that spells `SPONSKIP` with ASCII block letters. Include a readable `<h1>` for screen readers and search indexing.
- [x] Use a short factual promise: known sponsor segments are skipped; caption fallback depends on available captions. Keep the `v1.0.1` ZIP CTA and Store-coming-soon state.
- [x] Move the existing interactive demo below the hero without changing its DOM IDs or JavaScript behavior.

### Task 2: Atmospheric backdrop and visual system

**Files:** Modify `index.html` styles and add a scoped script in the same file.

- [x] Add `<canvas id="asciiField" aria-hidden="true"></canvas>` behind the hero content.
- [x] Draw a sparse glyph grid with a dark red palette and a faint moving signal band; cap at about 15 frames per second, pause when the page is hidden or hero is offscreen, and draw one still frame for reduced motion.
- [x] Style the wordmark, headline, CTA, and first supporting section with careful contrast, thin signal lines, and restrained mono labels. Keep the ASCII art nonselectable and pointer transparent.
- [x] Provide responsive wordmark sizing or a compact arrangement below the mobile breakpoint with no horizontal overflow.

### Task 3: Verify and refine

**Files:** `index.html`, only if checks reveal issues.

- [x] Run `npx --yes html-validate index.html`; expected result: no errors.
- [x] Serve the static directory and inspect at desktop and mobile widths. Confirm the brand art, headline, CTA, and demo are visible and legible; no horizontal scrolling.
- [x] Check reduced motion and hidden-tab behavior; confirm backdrop becomes still/stops.
- [x] Check release/privacy/support links and replay the existing demo.

## Self-review

The plan covers the wordmark, backdrop, Cloudflare-inspired space, responsive layout, motion controls, accurate copy, and unchanged install/demo flow. No new dependency, account, or backend is required.
