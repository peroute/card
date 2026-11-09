# card

Digital business card for **Mahmoud Bouassida**, CEO of Sté Yasmine de développement et exploitation agricole: **https://peroute.github.io/card/**

## ⚠️ Do not break the printed QR code

`qr.png` / `qr.svg` encode exactly `https://peroute.github.io/card/`. That URL is printed and can't change, so:

- **Do not rename this repo** (`card`).
- **Do not rename the GitHub account** (`peroute`). GitHub does not keep Pages URLs working after an account rename.
- **Do not delete the repo, make it private, or turn off GitHub Pages** (Settings → Pages → Deploy from branch `main` / root).
- **Keep `index.html` at the repo root.** If you move it, the URL returns a 404.

You can change everything else (page content, contact file, design) at any time. The QR code stays valid.

## Files

| File | What it is |
|---|---|
| `index.html` | The page. Self-contained, inline CSS and SVG. A small inline script handles the FR / AR / EN switch (the page still works in English without it). |
| `mahmoud.vcf` | vCard 3.0 contact file used by the "Save contact" button. |
| `og.jpg` | 1200×630 preview image shown when the link is shared on WhatsApp, LinkedIn, iMessage. |
| `qr.png` | 1480×1480 QR code (error correction Q, 4-module quiet zone). |
| `qr.svg` | Vector version of the same QR code. Use this for printing. |
| `.gitattributes` | Keeps `.vcf` line endings as CRLF (required by the vCard spec). |
| `.nojekyll` | Makes GitHub Pages serve the files as-is, without running Jekyll. |

## Editing the info

Most info is in **two places**. When you change something, update **both** files:

| Info | `index.html` | `mahmoud.vcf` |
|---|---|---|
| Name | `<title>`, `<h1>` | `N:Last;First;;;` and `FN:First Last` |
| Role / specialties / crops | the `T = { en, fr, ar }` table in the `<script>` (all three languages) **and** the English fallback text in the HTML, plus the `og:` and `description` meta tags | `TITLE:`, `NOTE:` |
| Company | `<p class="company">` (kept in French in every language) and the `og:`/`description` meta tags | `ORG;CHARSET=UTF-8:` |
| Location | `where` in the `T` table (all three languages) and the HTML fallback | `ADR;TYPE=WORK:;;;City;Region;;Country` |
| Phone | `https://wa.me/<digits>` and `tel:+<digits>` | `TEL;TYPE=CELL:+...` |
| Email | `href="mailto:..."` | `EMAIL;TYPE=INTERNET:` |

To use a photo instead of the "MB" monogram, add `photo.jpg` (square, at least 300×300) and follow the `PHOTO:` comment in `index.html`.
If you change the name or role, regenerate `og.jpg` so link previews match.

### Rules for `mahmoud.vcf`

- Keep `BEGIN:VCARD` first, `VERSION:3.0` second, and `END:VCARD` last.
- One field per line, no blank lines.
- Phone numbers go in international format: `+` and country code, then the digits, no spaces.
- In `NOTE:` and other text fields, write each comma as `\,` (that's how the vCard format escapes commas).
- Keep `;CHARSET=UTF-8` on lines with accented letters (é, à…) so every phone decodes them correctly.
- Line endings must be CRLF. `.gitattributes` already stops git from converting them. If you edit in VS Code, check the bottom-right corner shows `CRLF`.
- Don't edit it with `sed -i` from Git Bash on Windows: it silently strips the CR characters. Use an editor or Python.

### Publishing a change

```bash
git add -A
git commit -m "Update contact info"
git push
```

GitHub Pages redeploys in about a minute. Phones may have the old version cached; reload the page to see the new one.

## Regenerating the QR code (you should never need to)

```bash
pip install segno
python -c "import segno; q=segno.make('https://peroute.github.io/card/', error='q', micro=False); q.save('qr.png', scale=40, border=4); q.save('qr.svg', scale=10, border=4)"
```

No analytics, trackers, or third-party services.
