# Resume source

`resume.html` is the source; `../resume.pdf` is what the site serves and what
people actually read. The HTML is print-styled — `@page { size: letter; margin: 0 }`
plus the body's own padding — so it renders to a single letter page with no
browser header or footer.

It lives here, in the same repo as the PDF, on purpose. The two were previously
in different places, with the source untracked, so the only copy of the source
sat on one disk and nothing stopped it drifting from the committed PDF.

## Rebuilding the PDF

Any Chromium prints it identically — the committed PDF's producer is
`Skia/PDF`, which is Chrome's engine:

```bash
"/Applications/Google Chrome.app/Contents/MacOS/Google Chrome" \
  --headless --disable-gpu --no-pdf-header-footer \
  --print-to-pdf=resume.pdf \
  "file://$PWD/resume-src/resume.html"
```

Then check it is still one page before committing — the entries are written to
fill a page and a wrapped line is the usual way a change spills onto a second:

```bash
python3 -c "import re,io; print(len(re.findall(rb'/Type\s*/Page[^s]', io.open('resume.pdf','rb').read())), 'page(s)')"
```
