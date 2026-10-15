# Sponskip ASCII Homepage Design

**Date:** 2026-10-15  
**Status:** Design approved in conversation; written spec awaiting Blake's review  
**Scope:** `index.html` in the Sponskip GitHub Pages repository.

## Goal

Make Sponskip's homepage feel like a focused, futuristic product launch. The word **SPONSKIP** is the signature ASCII artwork, while a restrained field of characters creates depth behind the hero. Visitors can still immediately tell what the extension does and find the current GitHub release.

## References and Interpretation

- Blake requested ASCII art and pointed to the [21st.dev ASCII community](https://21st.dev/community/ascii). Use its [Code Rain](https://21st.dev/community/ascii/code-rain-14a45fc1-ebee-47a4-b8d4-772d084815d4) and [Schematic](https://21st.dev/community/ascii/blueprint-78b6004b-6c7c-4b2d-b4d7-648c4f28f5cb) recipes as visual inspiration, not as a dependency or an unlicensed copy.
- Blake's Cloudflare reference means a large, confident hero field with generous space, clear type and CTA, and subtle texture. Sponskip retains its own dark/red identity rather than Cloudflare's orange branding.

## Page Design

1. Keep the compact navigation and release links. Give the hero more height and room to breathe.
2. Render a large, crisp, monospaced **SPONSKIP** ASCII wordmark as the main visual. Use a fixed handcrafted character pattern so the lettering is deliberate and reliable. It should be readable on desktop, then scale or switch to a compact arrangement on narrow screens without horizontal scrolling.
3. Behind the wordmark, render a low-contrast field of small ASCII glyphs. The field can slowly drift or respond to a passing sweep. A red signal band cuts through part of the field to suggest a sponsor segment being skipped. Keep the center calm enough for the copy and CTA to read immediately.
4. Place a real HTML heading, a concise factual product explanation, and the current `v1.0.1` GitHub ZIP CTA in front of the art. The Chrome Web Store remains clearly marked as coming soon. Avoid absolute detection claims such as “every” sponsor read.
5. Carry the new visual language into the first product section and demo with thin signal lines, monospaced labels, and restrained red highlights. Keep the working interactive demo, installation steps, privacy details, FAQ, and footer accessible.

## Implementation Shape

- Keep the site as static HTML/CSS/JavaScript. No React conversion, build step, account, or third-party animation runtime.
- The ASCII wordmark is static text. The background is decorative and can be rendered with a single canvas or `<pre>` layer at a capped frame rate. It pauses when offscreen or when the tab is hidden.
- Under `prefers-reduced-motion: reduce`, show one still frame. Mark decorative art `aria-hidden="true"` and prevent text selection. The real heading remains readable by assistive technology and search engines.
- Preserve the site’s existing URLs, release version, and privacy claims unless a source check requires a copy correction.

## Verification

- Preview at desktop and mobile widths, checking ASCII lettering, line wrapping, CTA prominence, contrast, and no horizontal overflow.
- Confirm the animated backdrop does not obscure text, stops when hidden, and respects reduced motion.
- Validate HTML and check the release, privacy, support, and navigation links. Confirm the interactive demo still works.

## Delivery

Implement and review the updated page locally. Publish only after the page is visually verified and the release and Store status are accurate.
