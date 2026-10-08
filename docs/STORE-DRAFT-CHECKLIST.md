# Chrome Web Store draft checklist

Complete each item from the current extension source and the Chrome Web Store Developer Dashboard. Save a draft with deferred publishing; do not submit it for review until Blake explicitly approves submission.

## Account and package

- [ ] Chrome Web Store developer account is enrolled.
- [ ] `sponskip-v1.0.1.zip` passes `node scripts/validate-package.mjs` before upload.
- [ ] ZIP contains `manifest.json` at its root.
- [ ] Package SHA-256 is recorded: `PENDING_PACKAGE_BUILD`.

## Store listing

- [ ] Short description: `Skip sponsor and self-promotion segments on YouTube.`
- [ ] Detailed description is copied from `sponskip/sponskip` `docs/STORE-LISTING.md`.
- [ ] Category: Productivity.
- [ ] Language: English (United States).
- [ ] 128×128 icon uploaded.
- [ ] Truthful screenshots added: popup with a public SponsorBlock result, options defaults, and privacy page.
- [ ] Support URL: `https://github.com/sponskip/sponskip/issues`.
- [ ] Privacy policy URL: `https://sponskip.com/privacy.html`.

## Privacy and distribution

- [ ] Single purpose: Skip sponsor and self-promotion segments on YouTube.
- [ ] YouTube host permission is justified as page access and playback control.
- [ ] SponsorBlock host permission is justified as retrieval of public segment timestamps for the current video.
- [ ] Local storage is declared for user controls, cached segments, and aggregate skip statistics.
- [ ] No Sponskip-operated backend or analytics is declared.
- [ ] Free pricing and intended distribution regions are selected.

## Save evidence

- [ ] Chrome Web Store item URL: `PENDING_DRAFT_URL`.
- [ ] Screenshot filenames: `PENDING_SCREENSHOTS`.
- [ ] Deferred publishing is selected.
- [ ] Draft is saved without clicking **Submit for review**.
