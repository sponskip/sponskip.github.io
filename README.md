# Sponskip

Sponskip is a zero-latency, client-side Chrome extension that automatically detects and jumps past embedded creator promotions and disguised sponsor reads on YouTube videos and podcasts.

It skips known sponsor segments without relying on heavy models or cloud analytics. Sponskip operates entirely in your browser using local extension logic and SponsorBlock-compatible segment data. 

## Features
- **Client-Side Only**: Runs entirely in local Chrome memory. Zero browsing history leaves your machine.
- **Backward Trace Detection**: Detects pitch clusters and traces backwards to catch disguised setups.
- **Zero Audio Transcription Lag**: Reads YouTube's signed caption stream instantly.
- **No Analytics**: 100% private. No external tracking.

## Installation

**Chrome Web Store:**
*(Store listing pending review - link will be updated soon)*

**Developer Installation (Unpacked):**
1. Download or clone this repository: `git clone https://github.com/sponskip/sponskip.git`
2. Navigate to `chrome://extensions` in Chrome and toggle on **Developer mode**.
3. Click **Load unpacked**, select the `sponskip` folder.

## Privacy Policy
Sponskip does not collect, store, or transmit your personal data, viewing history, or analytics. The only external request made is to retrieve public SponsorBlock segment data using the ID of the video you are currently watching. All caption analysis and sponsor detection occurs locally on your machine.

## Support & Contributing
Please use GitHub Issues to report bugs or request features. When submitting PRs, ensure you adhere to the privacy-first architecture (no external calls, analytics, or trackers).

## License
MIT License
