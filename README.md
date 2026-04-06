# Elite Properties Signature Assets

This repository hosts the HTML signature template AND its image assets (logo
and icons) for the standardized Elite Properties email signature. Files are
served via GitHub Pages at `https://eliteprop.github.io/signature-assets/`
and accessed by employees during signature installation.

## Why GitHub Pages?

The signature originally embedded all images as base64 inside the HTML, which
worked but produced an ~80 KB signature file. We tried hosting the assets on
the WordPress site at eliteprop.com instead, but Gmail's image proxy
(`googleusercontent.com`) couldn't reliably fetch them — the cause was never
fully diagnosed but appeared to be something at the host or network edge,
since direct curl requests succeeded with both browser and Google User-Agent
strings.

GitHub Pages solved the problem because:

- It's free and served from Fastly's global CDN.
- Google trusts and freely fetches from `*.github.io`, so Gmail's image proxy
  has no trouble caching the assets.
- Updates are simple — commit a new file, and every signature in the company
  picks up the change on the next email view (subject to recipient image
  caches).
- It eliminates ~60 KB of base64 from every signed email.

We also moved the HTML signature template itself into this repo (instead of
distributing it as a file in a network folder) so that template updates can
be made centrally without anyone needing to redownload anything.

## Files in this repo

| File | Purpose |
|------|---------|
| `elite-signature-template.html` | The signature template employees copy from. Served as a web page. |
| `elite-logo.png` | Main Elite Properties circular logo, displayed at 130×130 (file is 260×260 PNG with transparent background) |
| `phone.png` | Phone handset icon (40×40 PNG, displayed at 16×16) |
| `email-icon.png` | Envelope icon (40×40 PNG, displayed at 16×16) |
| `address-icon.png` | Map pin icon (40×40 PNG, displayed at 16×16) |
| `website-icon.png` | Globe icon (40×40 PNG, displayed at 16×16) |
| `office-hours-icon.png` | Clock icon (40×40 PNG, displayed at 16×16) |

All icons are monochrome (#333333) on transparent background, generated from
hand-drawn SVG paths and rasterized to PNG. SVG sources are not stored here —
if you need to recreate them, see "Recreating icons" below.

## How employees use this repo

Employees do NOT clone this repo or download files from it directly. Instead,
the installation guide they receive points them to:

```
https://eliteprop.github.io/signature-assets/elite-signature-template.html
```

When they open that URL in a browser, GitHub Pages serves the HTML file as a
rendered web page (with the logo and icons loaded from the same repo). They
copy the rendered signature, paste it into Outlook's signature editor, and
type their own information over the placeholder text.

## ⚠️ DO NOT change these things

The following will break every employee's signature company-wide:

- **Don't rename files.** The signature template references each image by
  its exact filename, and the installation guide references the template
  HTML file by its exact filename.
- **Don't make this repo private.** GitHub Pages requires public repos on
  free accounts, and the signatures need anonymous public access to the URLs.
- **Don't delete the repo or transfer it to a different org/user.** The URL
  path `eliteprop.github.io/signature-assets/` is baked into every installed
  signature and into the installation guide.
- **Don't disable GitHub Pages** in the repo's Settings → Pages section.

If you need to make any of these changes, you'll have to coordinate a
company-wide signature reinstall, which is a significant undertaking.

## Safe things to change

- **Updating the logo image.** Replace `elite-logo.png` with a new file of
  the same name. Keep the same aspect ratio (square) and similar dimensions
  (260×260 recommended for retina sharpness at 130×130 display size). Be
  aware that some recipients may see the cached old logo for some time after
  an update — Gmail in particular caches images aggressively.
- **Updating an icon.** Same approach — replace the file with a new one of
  the same name. Keep dimensions consistent (40×40 PNG for 16×16 display).
- **Updating the HTML template.** Edit `elite-signature-template.html`
  directly. Changes take effect immediately for any new signature
  installations (existing installed signatures are not affected — those are
  copies that live in each employee's Outlook profile).
- **Adding new assets** for a new template version. Use a new filename so it
  doesn't affect the existing signature.

## Where the installation guide lives

The Word document installation guide (`Elite-Signature-Installation-Guide.docx`)
that employees follow is maintained separately, in a network folder along
with the README.txt. As of this writing, it should be in the IT documentation
section of Elite Properties' shared storage.

## Recreating icons (if ever needed)

The icons were generated using Python with the cairosvg library, rendering
hand-written SVG paths to 40×40 PNGs. Each icon uses fill color `#333333`.
If you need to regenerate them, you can either:

1. Find the original SVG sources (check Roger's archive — they were generated
   during the original signature build in early 2026).
2. Use any icon library with a similar visual style (Material Icons,
   Feather Icons, Heroicons) and export as 40×40 PNGs with `#333333` fill.
3. Ask an LLM to generate SVG icons matching the existing style and rasterize
   them with cairosvg or any SVG-to-PNG converter.

The visual style is intentionally simple — solid monochrome silhouettes,
no outlines, no gradients — so they read clearly at 16×16.

## Testing changes

After updating any file:

1. Wait a minute or two for GitHub Pages to redeploy (check the Actions tab
   for the deploy status).
2. Open the file's URL directly in an incognito browser window to confirm
   the new version is live.
3. For HTML template changes, also test the full flow: open the URL, copy,
   paste into a fresh Outlook signature, and verify the result. Send a test
   email to a Gmail account and check it on mobile too.

## Contact

Originally set up by Roger Rouse in early 2026 for Elite Properties, Inc.
For questions about the signature design or the original build process,
check the IT documentation or contact whoever currently handles IT.
