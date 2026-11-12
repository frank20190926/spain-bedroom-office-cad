# Dormitorio 3 — dual-option delivery archive

Static, self-hosted architectural drawing site. Open `index.html` through an HTTP server; no build step or third-party frontend dependencies.

- `?opt=a`: Option A / REV.06A / 3.19 m³ net new joinery.
- `?opt=b`: Option B / REV.06B / 3.13 m³ net new joinery / 60% ground-level reach.
- Both options remain visible in the overview; the selected option controls the four-sheet gallery and download section.
- Select any sheet to open its original. Previous/next buttons and arrow keys navigate; “View 100%” shows native resolution; Esc closes and returns focus.

## Files

```text
index.html
404.html                         archived Blender-file recovery link
assets/fonts/                    self-hosted typefaces
option-a/                        option-b/ has the same structure
  deliverables/BD01.jpg … BD04.jpg
  deliverables/BD01_1200.jpg … BD04_1200.jpg
  exports/Bedroom_Office_10.54sqm_OptionA_Carpenter_A3.pdf
  exports/Bedroom_Office_10.54sqm_OptionA_Master_Pack.zip
  manifest.json                  file sizes and SHA-256 hashes
deliverables/                    retained REV.05C images / legacy URLs
exports/                         retained REV.05C main downloads / legacy URLs
```

New full-size boards and packs are copied without modification from the formal proposal directories. Previews are 1200 × 900 JPEGs. Each PDF has four landscape A3 pages. The ZIP includes DWG, DXF and Blender files.

## Refresh after a proposal release

The requested helper lives **outside this repository**, at `../sync_gh_dual.py`.

```sh
python3 ../sync_gh_dual.py --option b --check
python3 ../sync_gh_dual.py --option b
# Refresh both completed proposals:
python3 ../sync_gh_dual.py
```

Uses existing Pillow and pypdf. `--source` and `--repo` override the local defaults; see `--help`. Finish rendering, composing the PDF and rebuilding the master ZIP before syncing. The helper checks four-page A3 dimensions, 3200 × 2400 originals, ZIP CRC and required CAD/model entries. Pack-contained boards and PDF must exactly match the external release files. A stale pack stops the refresh before changing the site. Source changes during copying are rejected. All selected options are staged before replacement; a replacement failure rolls back. Run one sync at a time, with no concurrent edits inside the generated `option-a/` / `option-b/` directories. These directories are replaced as complete generated units.

## Backward compatibility and size

The old page's root PDF, master ZIP, DWG, DXF and board links retain their original REV.05C contents. They are not silently reassigned to a new design. Its `#files` anchor is retained.

Only the redundant `exports/Bedroom_Office_10.54sqm_3D.blend` is removed (22,139,729 bytes). Its SHA-256 matched `model/Bedroom_Office_Dormitorio3.blend` inside the retained legacy ZIP. On GitHub Pages, `404.html` recognizes that historical address and offers the original pack. This is an archive recovery page, not an HTTP redirect or a direct BLEND response. Main legacy download URLs continue to resolve directly. Local `python3 -m http.server` does not serve custom 404 pages automatically.

Working files total approximately 167 MB, including the legacy archive; largest file is approximately 34 MB. Existing `.git` history is untouched, so deleting the duplicate does not reduce historical Git objects. No commit, history rewrite, push or deployment is performed.

## Local verification (2026-11-12)

- A/B release validation, ZIP CRC and all eight board/PDF source comparisons passed.
- Browser checks passed: direct option URLs, switching, browser Back, correct downloads, cross-option overview viewer, next sheet, 100% native 3200 px width, Escape and focus restoration.
- Desktop and 390 px mobile layouts inspected; no horizontal page overflow.
- Current HTML file references and `git diff --check` passed; independent static and screenshot review passed.
- Browser checks cover the local site. Hosted GitHub Pages behavior is not deployed or verified.
