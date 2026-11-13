# heirloom

Marketing landing page for **Heirloom**, the family-photo restoration studio
(`~/Projects/personal/heirloom-studio (repo bsc2fast/heirloom-studio)`). Target URL: **heirloom.sankhacooray.com**, served by a
Cloudflare Worker (static assets only), the same setup as `aimeter` and `cook`.

No source / GitHub link on the page: the studio is headed for Cloud Run behind a login gate.
The single CTA, **Request access**, scrolls to the shared sankhacooray.com contact form.

## Files

| Path | Purpose |
|------|---------|
| `public/index.html` | The whole page; CSS, screen-fit JS, particle field and theme switch inline |
| `public/og.png` | 1200×630 share image, rendered from `og/og.html` |
| `public/icon*.png`, `icon.svg`, `apple-touch-icon.png`, `manifest.webmanifest` | Icons + PWA manifest |
| `public/_headers` | 30-day cache for PNG/SVG |
| `wrangler.toml` | Worker config + custom domain |

## Share image and icons

```
C="/Applications/Google Chrome.app/Contents/MacOS/Google Chrome"
"$C" --headless=new --hide-scrollbars --force-device-scale-factor=1 --window-size=1200,630 \
  --virtual-time-budget=5000 --screenshot=public/og.png "file://$PWD/og/og.html"
for s in 192 512; do "$C" --headless=new --hide-scrollbars --force-device-scale-factor=1 \
  --window-size=$s,$s --screenshot=public/icon-$s.png "file://$PWD/og/icon.html"; done
"$C" --headless=new --hide-scrollbars --window-size=512,512 --screenshot=public/icon-maskable-512.png "file://$PWD/og/icon.html?maskable"
"$C" --headless=new --hide-scrollbars --window-size=180,180 --screenshot=public/apple-touch-icon.png "file://$PWD/og/icon.html"
```
Bump `?v=N` on the `og:image` / `twitter:image` URLs after changing the share image.

## Preview / deploy

```
python3 -m http.server 4796 --bind 127.0.0.1 -d public
CLOUDFLARE_ACCOUNT_ID=9a1da48a0f61e7cebcba39517856d87e npx wrangler deploy
```
After deploying, curl the page, `og.png` and the icons to confirm 200s and the right types.
