# vendor

Third-party libraries served from this domain instead of a CDN.

| file | version | source |
|---|---|---|
| `bootstrap-5.3.8.min.css` | 5.3.8 | `https://cdn.jsdelivr.net/npm/bootstrap@5.3.8/dist/css/bootstrap.min.css` |
| `bootstrap-5.3.8.bundle.min.js` | 5.3.8 | `https://cdn.jsdelivr.net/npm/bootstrap@5.3.8/dist/js/bootstrap.bundle.min.js` |
| `fontawesome-6.7.2/css/fontawesome.min.css` | 6.7.2 | `https://cdn.jsdelivr.net/npm/@fortawesome/fontawesome-free@6.7.2/css/fontawesome.min.css` |
| `fontawesome-6.7.2/css/solid.min.css` | 6.7.2 | same package, `css/solid.min.css` |
| `fontawesome-6.7.2/webfonts/fa-solid-900.woff2` | 6.7.2 | same package, `webfonts/fa-solid-900.woff2` |

## Why they left the CDN

The page used to load Bootstrap 5.3.3 from jsdelivr and Font Awesome
**6.0.0-beta3** from cdnjs, neither with an `integrity` attribute. Two problems,
both gone now that the files are here: a pre-release pinned in production for
years, and two hosts that could execute any code on this domain. Served from the
same origin, the file is exactly what this repository contains.

With no external host left, the CSP in `_headers` is back to `'self'`.

## Font Awesome: only what is used

Every icon on the page is `fas` (solid), so only the solid family was brought in:
`fontawesome.min.css` (the base) plus `solid.min.css` (the `@font-face`), and one
font file.

`solid.min.css` lists `fa-solid-900.ttf` as a fallback after the woff2. That file
is **not** here on purpose: a browser picks the first format it supports, and
woff2 has been universally supported since 2016, so the ttf is never requested.
Leaving it out saves 416 KB.

The folder is named with the version so an upgrade shows up clearly in the diff.

## How to update

Download the new version from jsdelivr, replace the files, update the paths in
`index.html`, and run the validation before publishing.
