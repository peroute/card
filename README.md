# card

My digital business card: **https://peroute.github.io/card/**

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
| `index.html` | The page. Self-contained, inline CSS, no JS. |
| `hedi.vcf` | vCard 3.0 contact file used by the "Save contact" button. |
| `qr.png` | 1480×1480 QR code (error correction Q, 4-module quiet zone). |
| `qr.svg` | Vector version of the same QR code. Use this for printing. |
| `.gitattributes` | Keeps `hedi.vcf` line endings as CRLF (required by the vCard spec). |
| `.nojekyll` | Makes GitHub Pages serve the files as-is, without running Jekyll. |

## Editing my info

Your info is in **two places**. When you change something, update **both** files:

| Info | `index.html` | `hedi.vcf` |
|---|---|---|
| Name | `<title>`, `<h1>`, avatar initials `HB` | `N:Last;First;;;` and `FN:First Last` |
| Title line | `<p class="title">` and the `<meta name="description">` | `ORG:` and `TITLE:` |
| Phone | `href="tel:+1..."` (no spaces) | `TEL;TYPE=CELL:+1...` |
| Email | Uncomment the `EMAIL` block and set `mailto:` | Add a line `EMAIL;TYPE=INTERNET:you@example.com` |
| LinkedIn | LinkedIn `<a href>` | `item1.URL:` |

> The phone number is currently a **placeholder** (`+1 555-555-0100`). Replace it in both files.

### Rules for `hedi.vcf`

- Keep `BEGIN:VCARD` first, `VERSION:3.0` second, and `END:VCARD` last.
- One field per line, no blank lines.
- Phone numbers go in international format: `+1` followed by the digits.
- Line endings must be CRLF. `.gitattributes` already stops git from converting them. If you edit in VS Code, check the bottom-right corner shows `CRLF`.

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
