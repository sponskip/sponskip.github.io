# Sponskip Public-Release Design

**Date:** 2026-10-07  
**Status:** Approved blueprint — awaiting written-spec review  
**Scope:** GitHub presentation, product website, Chrome Web Store draft readiness.

## Goal

Present Sponskip as a credible, privacy-respecting Chrome extension: the Chrome Web Store is the primary installation route, while GitHub provides transparent source, support, releases, and project context. A visitor should understand the product, its limits, and where to get help in under a minute.

## Product Positioning

Sponskip skips known sponsor segments in YouTube videos using locally executed extension logic and SponsorBlock-compatible segment data. The public copy must avoid unsupported guarantees (for example, universal detection, exact latency, or "disguised ads" detection) and must never imply affiliation with YouTube or SponsorBlock.

The primary conversion is: **Install from Chrome Web Store**. The fallback is clear source installation for developers. GitHub is not positioned as the default consumer installation path.

## 1. GitHub Repository

The extension repository becomes the canonical technical home.

- A concise README introduces the problem, supported scope, install options, privacy posture, architecture summary, and support path.
- Repository metadata (description, topics, homepage, social-preview image) matches the website and Store listing.
- Documentation covers privacy, support, contribution expectations, security reporting, and third-party attribution/licensing.
- Issue and pull-request templates capture actionable reports without collecting browsing history or unnecessary personal data.
- Releases use semver tags, include a downloadable Chrome extension ZIP, and record user-visible changes.
- The repository’s visible files do not contain production secrets, user data, or misleading performance claims.

## 2. Product Website

The existing GitHub Pages site becomes a focused product page rather than a developer download page.

### Page structure

1. Compact header with logo, GitHub, and primary Store CTA.
2. Hero: one factual promise, supported-platform cue, and Store CTA; source installation is secondary.
3. Product explanation: how a known sponsor segment is identified and skipped, expressed without claiming universal coverage.
4. Benefits: less interruption, local extension operation, and user control.
5. Trust block: privacy summary, non-affiliation notice, and data-source attribution.
6. Simple installation / FAQ section that switches between Store and developer setup depending on release state.
7. Support/footer links: GitHub, privacy policy, support/issues, terms or license, and copyright.

### Visual direction

Retain the dark, sharp red visual language but replace generic dashboard-style mockups with clear extension-state visuals. Use accessible contrast, a single CTA hierarchy, responsive mobile spacing, and no invented statistics. Store screenshots can later be reused on the website where appropriate.

## 3. Publication Readiness

The Chrome Web Store draft should be assembled only after the product claims and privacy policy are published.

- Validate `manifest.json`: name, version, description, permissions, host permissions, icons, and externally hosted assets.
- Produce a release ZIP with exactly the files needed by the extension; exclude development files and credentials.
- Write Store listing content: short description, detailed description, category, language, support URL, privacy-policy URL, and permission justifications.
- Provide compliant promotional artwork: icon and at least one truthful screenshot of the extension in use. Promotional tiles only if required by the selected listing type.
- Complete privacy disclosures based on the actual extension behavior—not aspirational claims.
- Test the packaged extension in a clean Chrome profile before draft submission.

## Boundaries and Assumptions

- This work prepares a Store draft; it does not publish or submit it for review without Blake’s explicit authorization.
- The official Store URL is unknown until the draft is created. Until then, the website uses a clearly marked pending state or a non-primary placeholder that cannot mislead visitors.
- Product claims will be verified against the extension source before use. If the source cannot support a claim, copy is narrowed rather than functionality invented.
- GitHub ownership and publishing permissions remain under the `sponskip` organization; any remote mutations will be performed only after local checks pass.

## Verification

- HTML validation and responsive-browser check for the site.
- Link check for all primary CTAs and legal/support links.
- Repository hygiene review: clean status, understandable onboarding, appropriate ignored files, license and disclosure presence.
- Extension packaging audit and a clean-profile load test.
- Store-draft checklist with every required field, asset, and declaration marked as verified or explicitly outstanding.

## Delivery Sequence

1. Audit the extension source and current GitHub repositories to ground claims.
2. Upgrade GitHub repository presentation and documentation.
3. Revamp and verify the product site, then deploy it to GitHub Pages.
4. Package and test the extension.
5. Assemble all material necessary for a Chrome Web Store draft and provide the final submission checklist.
