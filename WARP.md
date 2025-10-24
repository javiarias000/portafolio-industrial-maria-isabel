# WARP.md

This file provides guidance to WARP (warp.dev) when working with code in this repository.

## Commands

- Serve locally (Python, built-in on most systems):
```bash path=null start=null
python3 -m http.server 8000
# Then open http://localhost:8000
```
- Serve locally (Node alternative):
```bash path=null start=null
npx serve .
# Or: npx http-server -c-1 .
```
- Build/lint/tests: this repo has no build system, no linter config, and no test runner configured.

## Architecture and structure

- Overview
  - Static site: `index.html`, `styles.css`, and an assets folder `portafolio/` (images). No bundler or package manager; JavaScript is inline in `index.html` and one external CDN (`html2pdf.js`).

- HTML (`index.html`)
  - Global navbar with anchor links to sections; a `.main-content` wrapper holds all sections so they can be exported to PDF without the fixed navbar/footer.
  - Sections:
    - `#home` (hero): headline and subtitle.
    - `#about`: two-column layout with `portafolio/Isabel.jpg` profile image and descriptive text.
    - `#skills`: three grouped skill columns; progress bars are set via inline `style="width: N%;"` on `.skill-progress` elements.
    - `#portfolio`: filter buttons (`Todos`, `Tesis`, `Mobiliario`, `Renderizado`) and a dynamically rendered `.gallery`.
    - `#contact`: basic contact details; static footer follows.
  - Inline JavaScript defines:
    - A `projects` array (title, category, description, img) used to render the gallery. Categories are space-separated strings and support multi-category items (e.g., "tesis mobiliario"). Images are referenced via relative paths under `portafolio/`.
    - `renderGallery(filter)` to populate `#project-gallery` based on the active filter button.
    - PDF export: a click handler on `#download-pdf` uses `html2pdf` to export `.main-content`, temporarily hides navbar/footer/button, and saves as `Portafolio_Ingeniera_Villena.pdf`.

- CSS (`styles.css`)
  - Dark theme with CSS variables (`:root`) for primary/secondary colors and backgrounds.
  - Layout styling for navbar, sections, grids, gallery cards, and contact info. Progress bars animate via CSS transition when widths are set.

- Assets (`portafolio/`)
  - Image files used by the gallery and profile. Filenames include spaces; all references are relative (e.g., `portafolio/Captura de pantalla ....png`). Ensure any new assets are placed here and referenced with the correct relative path.

## Notes for agents

- To add or modify portfolio items, edit the `projects` array in `index.html` (title, category tags, description, and `img` path under `portafolio/`).
- To change the exported PDF (filename, margins, target element), update the `options` object and the selector in the `#download-pdf` click handler in `index.html`.
