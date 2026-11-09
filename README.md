# card

Digital business card for **Mahmoud Bouassida**, agricultural expert and consultant: **https://peroute.github.io/card/**

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
| `index.html` | The page. Self-contained, inline CSS and SVG, no JS. |
| `mahmoud.vcf` | vCard 3.0 contact file used by the "Save contact" button. |
| `qr.png` | 1480×1480 QR code (error correction Q, 4-module quiet zone). |
| `qr.svg` | Vector version of the same QR code. Use this for printing. |
| `.gitattributes` | Keeps `.vcf` line endings as CRLF (required by the vCard spec). |
| `.nojekyll` | Makes GitHub Pages serve the files as-is, without running Jekyll. |

## Editing the info

Most info is in **two places**. When you change something, update **both** files:

| Info | `index.html` | `mahmoud.vcf` |
|---|---|---|
| Name | `<title>`, `<h1>` | `N:Last;First;;;` and `FN:First Last` |
| Position | `<p class="title">` and the `<meta name="description">` | `TITLE:` |
| Experience / crops | `<p class="exp">`, `<p class="crops">` | `NOTE:` |
| Phone | not shown on the page | `TEL;TYPE=CELL:+...` |
| Email | `href="mailto:..."` | `EMAIL;TYPE=INTERNET:` |

To add a company name, add a line `ORG:Company Name` to `mahmoud.vcf`.
To add a Call button back, copy the Email `<a class="btn">` block in `index.html` and change its link to `tel:+<digits>`.

### Rules for `mahmoud.vcf`

- Keep `BEGIN:VCARD` first, `VERSION:3.0` second, and `END:VCARD` last.
- One field per line, no blank lines.
- Phone numbers go in international format: `+` and country code, then the digits, no spaces.
- In `NOTE:` and other text fields, write each comma as `\,` (that's how the vCard format escapes commas).
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
