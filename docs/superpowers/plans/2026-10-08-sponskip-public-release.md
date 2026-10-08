# Sponskip Public Release Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Make Sponskip's public GitHub presence, website, extension package, and Chrome Web Store draft material consistent, trustworthy, and ready for review.

**Architecture:** Keep the extension as a plain Manifest V3 project. Extract the detection code into a pure script testable by Node, while Chrome messaging remains in the service worker and content script. Keep the website static and make all product claims traceable to the extension source.

**Tech Stack:** Manifest V3, vanilla JavaScript, Node.js built-in test runner, static HTML with Tailwind CDN, GitHub Pages, Chrome Web Store.

---

## Existing facts

- `sponskip/sponskip` is tagged `v1.0.0`; its manifest requests `storage`, `https://www.youtube.com/*`, and `https://sponsor.ajay.app/*`.
- The supported detection chain is SponsorBlock first, then local caption-keyword matching. No published claim may promise AI, universal coverage, approval, or latency.
- The Pages repository already has a dark/red visual refresh, a privacy page, contributor guidance, and a security policy. Preserve the look; correct claims and Store status.
- Save a Chrome Web Store draft with deferred publishing. Do not submit it for review.

## File map

### Extension repository

- Create `detector.js` for pure transcript detection.
- Create `test/detector.test.cjs` for deterministic Node tests.
- Create `scripts/validate-package.mjs` to validate manifest and ZIP root.
- Create `docs/PRIVACY.md`, `docs/STORE-LISTING.md`, `docs/RELEASE.md`, `CONTRIBUTING.md`, `SECURITY.md`, and `.github` issue/PR templates.
- Modify `manifest.json`, `background.js`, `content.js`, `popup.js`, `options.js`, `README.md`, `.gitignore`.

### Website repository

- Create `docs/STORE-DRAFT-CHECKLIST.md`.
- Modify `index.html`, `privacy.html`, `README.md`, `CONTRIBUTING.md`, and `SECURITY.md`.

## Task 1: Lock public claims to source behavior

**Files:** extension `manifest.json`, `README.md`, `docs/PRIVACY.md`, `docs/STORE-LISTING.md`.

- [ ] **Step 1: Write the factual data-flow matrix.**

Create `docs/STORE-LISTING.md` with:

```markdown
# Chrome Web Store Listing — Sponskip

## Single purpose

Sponskip skips SponsorBlock sponsor and self-promotion segments in YouTube videos and can locally inspect available captions to identify likely sponsor reads.

## Verified data behavior

| Data | Purpose | Destination |
| --- | --- | --- |
| YouTube page and video ID | Locate and control the video | Current YouTube tab; video ID is also used for the public SponsorBlock request. |
| Caption text | Local fallback matching | Extension memory only. |
| Settings and statistics | Remember controls and counts | `chrome.storage.local` only. |
| SponsorBlock request | Retrieve segment timestamps | `https://sponsor.ajay.app`; no Sponskip server receives data. |
```

- [ ] **Step 2: Prove the matrix against code.**

Run:

```bash
rg -n 'fetch\(|storage\.(local|sync)|host_permissions|caption|videoId' manifest.json background.js content.js popup.js options.js
```

Expected: every row in the document has a corresponding source location.

- [ ] **Step 3: Make the manifest description factual.**

Set its description to:

```json
"description": "Skip SponsorBlock sponsor segments and locally detected sponsor reads on YouTube."
```

- [ ] **Step 4: Replace unsupported README claims.**

Use:

```markdown
## What Sponskip does

1. Checks public SponsorBlock timestamps for `sponsor` and `selfpromo` segments.
2. If no public segment is available and captions are available, checks caption text locally for sponsor-read patterns.
3. Seeks the YouTube player past detected segments when auto-skip is enabled.

Caption-based detection is heuristic. It can miss sponsor reads and can occasionally identify a non-sponsor segment incorrectly. SponsorBlock coverage varies by video.
```

- [ ] **Step 5: Write `docs/PRIVACY.md`.** Include sections named **Data Sponskip accesses**, **Data Sponskip stores**, **Third-party request**, **No Sponskip analytics**, **Your controls**, and **No affiliation**. Do not claim SponsorBlock receives only a video ID or hides network identifiers.

- [ ] **Step 6: Confirm removed claims are gone.**

```bash
rg -n -i 'gemini|ai fallback|zero latency|100% private|disguised ads' README.md docs manifest.json background.js content.js
```

Expected: no unsupported claim remains.

- [ ] **Step 7: Commit.**

```bash
git add manifest.json README.md docs/PRIVACY.md docs/STORE-LISTING.md
git commit -m "docs: align public claims with behavior"
```

## Task 2: Align runtime behavior with the privacy document

**Files:** extension `detector.js`, `test/detector.test.cjs`, `background.js`, `content.js`, `popup.js`, `options.js`.

- [ ] **Step 1: Write a failing detector test.**

Create `test/detector.test.cjs`:

```js
const test = require('node:test');
const assert = require('node:assert/strict');
const { detectByKeywords } = require('../detector.js');

test('finds an explicit sponsor read and return phrase', () => {
  const transcript = '[10] This episode is brought to you by Acme.\n[20] Use promo code HELLO for 20% off.\n[42] Anyway, back to the interview.';
  assert.deepEqual(detectByKeywords(transcript), [{ start: 10, end: 42, reason: 'sponsor' }]);
});

test('does not mark an ordinary product discussion as sponsored', () => {
  assert.deepEqual(detectByKeywords('[10] This review compares VPN protocols.\n[30] The benchmark is complete.'), []);
});
```

- [ ] **Step 2: Confirm it fails.**

```bash
node --test test/detector.test.cjs
```

Expected: FAIL because `detector.js` does not exist.

- [ ] **Step 3: Extract the detector.** Move detection regexes and `detectByKeywords` from `background.js` into `detector.js`; preserve the `[{ start, end, reason }]` contract and end the file with:

```js
if (typeof module !== 'undefined') module.exports = { detectByKeywords };
```

Load it at the start of `background.js` with:

```js
importScripts('detector.js');
```

- [ ] **Step 4: Test the extraction.**

```bash
node --test test/detector.test.cjs
node --check detector.js && node --check background.js && node --check content.js
```

Expected: both tests and all syntax checks pass.

- [ ] **Step 5: Use only `chrome.storage.local`.** Replace every `chrome.storage.sync` call in `content.js`, `popup.js`, and `options.js`. Add `showToast` and `showBadgeCount` to the cached settings. Make the content toast honor `showToast`, and make `updateBadge(tabId, count, enabled)` clear the badge when `enabled` is false.

- [ ] **Step 6: Remove HTML injection paths.** Replace creator-name `innerHTML` interpolation in `popup.js` and `options.js` with elements built using `document.createElement` and `textContent`.

- [ ] **Step 7: Prove the privacy/runtime contract.**

```bash
rg -n 'storage\.sync|innerHTML.*channel|innerHTML.*name' background.js content.js popup.js options.js
```

Expected: no result.

- [ ] **Step 8: Commit.**

```bash
git add detector.js test/detector.test.cjs background.js content.js popup.js options.js
git commit -m "fix: align settings with privacy controls"
```

## Task 3: Build a package gate and release guide

**Files:** extension `scripts/validate-package.mjs`, `docs/RELEASE.md`, `.gitignore`.

- [ ] **Step 1: Write `scripts/validate-package.mjs`.** It must parse `manifest.json`; require Manifest V3, `name`, `version`, `description`, `action`, and 16/48/128 icon files; and reject archives with `manifest.json` below a parent directory. The only package root entries are `manifest.json`, `background.js`, `content.js`, `detector.js`, `popup.html`, `popup.js`, `options.html`, `options.js`, `icons`, and `LICENSE`.

- [ ] **Step 2: Verify the gate.**

```bash
node scripts/validate-package.mjs
```

Expected: exit 0 with a clear pass summary.

- [ ] **Step 3: Document and build the artifact.** `docs/RELEASE.md` must require a fresh Chrome profile, one known SponsorBlock skip, no-caption handling, every toggle, options reset, validator run, then package inspection. Build with:

```bash
node scripts/validate-package.mjs
stage_dir=$(mktemp -d)
cp manifest.json background.js content.js detector.js popup.html popup.js options.html options.js LICENSE "$stage_dir"
cp -R icons "$stage_dir/icons"
(cd "$stage_dir" && zip -qr "$OLDPWD/sponskip-v1.0.1.zip" .)
unzip -l sponskip-v1.0.1.zip
```

Expected: the manifest is at ZIP root; no source-control, tests, or docs are included.

- [ ] **Step 4: Add `sponskip-v*.zip` to `.gitignore`, then commit.**

```bash
git add scripts/validate-package.mjs docs/RELEASE.md .gitignore
git commit -m "build: add extension package validation"
```

## Task 4: Make the source repository official

**Files:** extension `README.md`, `CONTRIBUTING.md`, `SECURITY.md`, `.github/ISSUE_TEMPLATE/bug_report.yml`, `.github/ISSUE_TEMPLATE/feature_request.yml`, `.github/pull_request_template.md`, `LICENSE`.

- [ ] **Step 1: Replace the README opening.**

```markdown
# Sponskip

Skip known sponsor and self-promotion segments on YouTube, with local caption-based detection as a fallback.

Chrome Web Store availability will be linked here after the listing is approved. Until then, use the latest GitHub release or load the source directory unpacked.
```

- [ ] **Step 2: Add sections for How it works, Privacy, Limitations, Support, Contributing, Security, License, and Third-party services.** Link to `docs/PRIVACY.md` and credit SponsorBlock without implying affiliation.

- [ ] **Step 3: Preserve the current source-available license unless Blake changes the business decision.** Use “source-available” everywhere; if legal terms contradict it, stop and ask whether to use an OSI license or the protected source-available model.

- [ ] **Step 4: Add structured issue and PR intake.** The bug form requires Chrome version, extension version, expected/actual behavior, and warns reporters not to share private or unlisted video URLs. The PR template requires behavior, privacy, permission, and documentation checks. `CONTRIBUTING.md` forbids analytics and undeclared endpoints. `SECURITY.md` uses GitHub private vulnerability reporting or the repository owner’s profile, never a personal email.

- [ ] **Step 5: Check stale wording and commit.**

```bash
rg -n 'blakeb056/sponskip|Gemini|open source|zero latency|100% private' README.md CONTRIBUTING.md SECURITY.md docs .github
git add README.md LICENSE CONTRIBUTING.md SECURITY.md .github docs
git commit -m "docs: prepare public Sponskip repository"
```

Expected: no stale owner URL or unsupported marketing claim.

## Task 5: Make the website release-honest

**Files:** site `index.html`, `privacy.html`, `README.md`, `CONTRIBUTING.md`, `SECURITY.md`.

- [ ] **Step 1: Find every unverified status or absolute claim.**

```bash
rg -n -i 'in review|zero latency|100% client-side|disguised pitch|store listing' index.html privacy.html README.md
```

- [ ] **Step 2: Retain the visual design and release ZIP CTA.** Replace the false Store control with:

```html
<span class="text-xs text-gray-400">Chrome Web Store listing coming soon</span>
```

Add the actual Store URL only after the dashboard assigns it.

- [ ] **Step 3: Replace hero copy with:**

```html
<p class="text-base sm:text-xl text-gray-400 max-w-2xl mx-auto mb-10 leading-relaxed font-normal">
  Sponskip checks public SponsorBlock segments first, then can inspect available captions locally to spot likely sponsor reads on YouTube. Turn on auto-skip and it jumps past the segments it finds.
</p>
```

Add: “Caption-based detection is heuristic and may miss or misidentify segments.” near the demo.

- [ ] **Step 4: Make `privacy.html` exactly reflect Task 2.** State SponsorBlock receives the current video identifier to return public segments, preserve the YouTube/SponsorBlock non-affiliation notice, and route contact to GitHub support rather than a personal email.

- [ ] **Step 5: Validate and commit.**

```bash
npx --yes html-validate index.html privacy.html
rg -n 'href="https://github.com/sponskip/sponskip|href="privacy.html|sponsor.ajay.app' index.html privacy.html
git add index.html privacy.html README.md CONTRIBUTING.md SECURITY.md
git commit -m "fix: align website with release facts"
```

Expected: validation passes and privacy/support/source links are present.

## Task 6: Assemble the Chrome Web Store draft

**Files:** site `docs/STORE-DRAFT-CHECKLIST.md`; extension `docs/STORE-LISTING.md`; local `store-assets/` directory.

- [ ] **Step 1: Create the checklist.** Include developer account, ZIP upload, Store Listing copy, 128×128 icon, truthful screenshots, support URL, privacy URL, category/language, single purpose, each permission justification, distribution, and deferred publishing.

- [ ] **Step 2: Add final Store copy.** Use this short description: “Skip sponsor and self-promotion segments on YouTube.” Use this detailed description:

```markdown
Sponskip checks public SponsorBlock segments when you watch a YouTube video. When captions are available and no public segment is found, it can inspect those captions locally for likely sponsor reads. With auto-skip enabled, Sponskip moves playback past the segments it finds.

SponsorBlock coverage and caption-based detection vary by video. Sponskip is not affiliated with YouTube, Google, or SponsorBlock.
```

- [ ] **Step 3: Capture a clean-profile screenshot set.** Use a public video with a known SponsorBlock segment. Show the popup, options defaults, and website privacy page. Exclude account avatars, profile names, history, bookmarks, and unrelated tabs.

- [ ] **Step 4: Create a deferred-publishing draft.** Upload the validated ZIP, paste approved copy, use deployed `privacy.html`, answer the Privacy tab from the factual matrix, add screenshots, choose deferred publishing, and save. Do not click **Submit for review**.

- [ ] **Step 5: Record proof and commit safe documentation.** Record version, package SHA-256, Store item URL, and non-sensitive screenshot names in the checklist.

```bash
shasum -a 256 sponskip-v1.0.1.zip
git add docs/STORE-DRAFT-CHECKLIST.md
git commit -m "docs: add Chrome Web Store draft checklist"
```

## Task 7: Final verification and handoff

**Files:** both worktrees.

- [ ] **Step 1: Run the extension gate.**

```bash
node --test test/detector.test.cjs
node scripts/validate-package.mjs
node --check background.js && node --check content.js && node --check popup.js && node --check options.js
```

- [ ] **Step 2: Run the website gate.**

```bash
npx --yes html-validate index.html privacy.html
git diff --check
```

- [ ] **Step 3: In a clean profile, confirm SponsorBlock skip, no-caption behavior, auto-skip, toast, badge, sound, rescan, clear-data, and every site CTA.**

- [ ] **Step 4: Review upstream before sharing changes.** In both repositories run:

```bash
git fetch origin
git log --oneline HEAD..origin/main
git status --short --branch
```

- [ ] **Step 5: Deliver the branch names, validation results, package SHA-256, deployed website URL, Store draft URL, and the single final decision: whether Blake wants to submit the draft for review.**

## Plan self-review

- **Spec coverage:** Task 1 grounds every claim; Tasks 2–3 make privacy and packaging behavior true; Task 4 makes GitHub official; Task 5 preserves and corrects the site; Task 6 prepares the Store draft; Task 7 validates it.
- **External boundary:** only a saved draft is created; Store review submission requires a later explicit action.
- **Scope:** Two repositories support one release; each task produces independently verifiable output.

