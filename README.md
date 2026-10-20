# PT 原盘转载发布与智能查重助手

Tampermonkey userscript for moving untouched Blu-ray discs between private trackers. It reads a listing on site A, drops encodes / DVD / DIY / 禁转, asks site B whether the same disc is already there, and can fill B's upload form from the source page's BDInfo.

The installable file is [`dist/pt-bluray-assistant.user.js`](dist/pt-bluray-assistant.user.js). Source lives in `src/` and is bundled back into that single Greasemonkey script.

## What it does

On a torrent listing or detail page the script adds a panel:

- **预设** locks the current site's search form to movie full-disc categories.
- **拉取/查重** audits the open page, walks later pages (or the UNIT3D API when a key is set), and searches the destination site by IMDb id.
- A local **词库** remembers each successful check for the configured number of hours (default 24). The next visit repaints those rows without asking site B again.
- Rows that site B does not already have go into a **待发** queue. **转种** pulls BDInfo, screenshots, and an optional Douban synopsis, then opens the destination upload page and fills the form. **推qB** sends the `.torrent` to qBittorrent.

Supported page shapes are NexusPHP, UNIT3D, Gazelle, and the f3nix layout used by BeyondHD. The destination defaults to PTerClub and can be changed in settings.

## Pause and resume

暂停 keeps the queue and the词库. It also stores the listing that was already pulled (`pt_batch_checkpoint`, 12 hours, same site pair and same filters).

- The next click continues page fetching from the saved page instead of page 1.
- Titles with a fresh, successful词库 entry are logged once as **词库秒过** and are not requested again. The counter continues, so a stop at 46/1009 comes back as `[47/1009]`.
- Failed requests are not treated as finished, so the next run retries them.
- While that checkpoint exists, the词库 cap does not evict rows that belong to the open batch. A 500-entry cap used to delete the beginning of a 1000-title run, which made the next click look like a full restart.

The delay between requests still starts at the configured interval and adds the existing 3 second rest every 15 real requests. A 429 or other failure lengthens the next wait. Cached titles do not spend that delay.

## Build

```bash
npm install
npm test
npm run build
```

`npm test` covers the pure helpers and also builds `dist/pt-bluray-assistant.user.js`. Install that file in Tampermonkey. The metadata block is `src/userscript.meta.js`; esbuild prepends it to the bundle from `src/main.js`.
