---
name: image-paginator
description: Slices long images/screenshots into overlapping segments, adds sequence numbers, and auto-arranges them into a paginated PDF with gaps and page numbers. Supports multiple images.
license: MIT
metadata:
  author: WUDI
  version: "2.0"
---

# Image Paginator

## When to use this skill
Use this skill when the user provides one or more long images (like long chat screenshots, full webpage captures, or multiple screenshots in order) and wants to convert them into a well-formatted PDF document.

## Prerequisites
Required python packages: `Pillow`, `fpdf2`.

## How to use this skill
Execute the python script `scripts/slice_n_pdf.py` via the command line.

### Command Syntax
```bash
python scripts/slice_n_pdf.py <source1> [source2 ...] -d <output_dir> [OPTIONS]
```

### Required Arguments
* `sources` (positional): Path(s) to source image(s). Multiple images will be auto-resized to the same width and concatenated in order.
* `-d` / `--dest`: Directory to save the output PDF.

### Optional Arguments
* `-o` / `--output`: Name of the output PDF file (default: `output.pdf`).
* `--tile`: Height of each tile in pixels (default: `2000`).
* `--bleed`: Bleed / overlap in pixels (default: `200`).
* `--cols`: Grid columns (default: `2`).
* `--rows`: Grid rows (default: `2`).
* `--gutter`: Gap between cells in pixels (default: `40`).
* `--edge`: Page margin in pixels (default: `25`).
* `--no-numbers`: Omit page numbers at the bottom.
* `--clean`: Remove intermediate tile images after build.

### Key Features (v2.0)
1. **Multiple source images**: Pass multiple paths; images auto-resize to uniform width and concatenate vertically.
2. **Sequence numbers**: Each tile has a numbered badge (1, 2, 3…) on the top-left corner.
3. **Gaps between tiles**: Configurable gutter with light gray cell backgrounds for readability.
4. **Consistent page sizes**: All PDF pages share the same dimensions.
5. **Page numbers**: Each page has a centered `- N -` footer (disable with `--no-numbers`).
6. **Adaptive tail merging**: Short tail segments merge into the previous tile to avoid tiny orphans.

## ⚠️ Important Instructions for the Agent (Guardrails)
1. **Always use `--clean`** by default unless the user specifically asks to keep tiles.
2. **Absolute Paths**: Resolve `~` and relative paths to absolute paths before running.
3. **Multiple images**: When the user provides multiple images, pass all as positional arguments.
4. **DO NOT try to read the output PDF**: The result is binary. Just check STDOUT — if it says `✓ Done`, tell the user the file path.
5. **Always open the PDF** for the user after generation using `open <path>` (macOS).

## Examples

**Single long image:**
```bash
python scripts/slice_n_pdf.py "/Users/bob/Downloads/long_chat.png" \
  -d "/Users/bob/Desktop" -o "chat.pdf" --clean
```

**Multiple images with custom grid:**
```bash
python scripts/slice_n_pdf.py "/Users/bob/Desktop/1.jpg" "/Users/bob/Desktop/2.jpg" \
  -d "/Users/bob/Desktop" -o "combined.pdf" \
  --cols 2 --rows 2 --gutter 40 --clean
```

**Single column, larger tiles:**
```bash
python scripts/slice_n_pdf.py "/abs/path/webpage.jpg" \
  -d "./results" --cols 1 --rows 3 --tile 3000 --gutter 20 --clean
```
