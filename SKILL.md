---
name: bignbniyu-editable-vector
description: Convert user-provided scientific figures, biomedical schematics, diagrams, and infographics into clean, editable, low-node SVG vector artwork with grouped elements and optional vector PDF. Use when the user asks to vectorize, redraw, trace, reconstruct, or make an image editable in Illustrator/Inkscape.
---

# bigNBniyu_editable_vector

## Goal

Turn a supplied raster figure into a **clean, editable vector reconstruction**, not a crude automatic trace. The result should preserve the source figure's composition and scientific meaning while using smooth curves, clear boundaries, restrained path counts, and logical groups that can be edited independently in Adobe Illustrator, Inkscape, or Affinity Designer.

Follow the user's latest instruction first. If the user says to modify only selected details, change only those details and keep everything else unchanged.

## Non-negotiable quality criteria

1. **Editable vector artwork**
   - Deliver a real SVG containing vector paths, native shapes, gradients, strokes, and live text where practical.
   - Do not embed the source raster as the visible final artwork or use an image as a hidden backing layer.
   - A preview PNG may be included, but it is not the vector master.
   - Export a vector PDF when feasible; verify the PDF is not a rasterized screenshot.

2. **Low-node, smooth geometry**
   - Do not use raw auto-trace output as the final result.
   - Do not trace antialiasing, texture, noise, or every shadow band into separate contours.
   - Reconstruct shapes with a small number of intentional cubic Bézier segments.
   - Prefer one clean closed path per coherent silhouette; use additional paths only for meaningful inner boundaries, highlights, shadows, folds, or membranes.
   - Use circles/ellipses for round objects, native lines for straight connectors, and smooth open paths for arrows or filaments.
   - Use gradients or opacity for soft shading rather than many stacked colored outlines.
   - Avoid tiny stray paths, overlapping duplicate contours, jagged corners, and excessive anchor points.

3. **Logical grouping**
   - Every meaningful small element should be independently selectable or part of a clearly named group.
   - Use named SVG groups (`<g id="...">`) with readable, stable IDs.
   - Suggested hierarchy:
     - `background`
     - `heading-and-labels`
     - `timeline-and-connectors`
     - `panel-a`, `panel-b`, etc.
     - per-panel groups such as `cell-neuron`, `mitochondrion`, `protein-tufm`, `modification-label`, `arrow-import`
     - complex object subgroups such as `mitochondrion-outer-membrane`, `mitochondrion-inner-membrane`, `mitochondrion-cristae`
   - Avoid grouping the entire figure into one object or splitting every pixel fragment into its own object.
   - Keep labels, outlines, fills, and meaningful inner structures separate where this improves editing.

4. **Faithful composition**
   - Match the original canvas ratio, margins, panel positions, spacing, alignment, label placement, line weights, and color palette.
   - Preserve all text, symbols, arrows, units, abbreviations, scientific notation, and panel labels.
   - Do not rewrite scientific claims, normalize terminology, or silently correct source wording.
   - If text is uncertain, inspect the source at high zoom and use OCR only as a helper; visually verify it.
   - Do not invent detail that is not supported by the reference. Where a source feature is ambiguous, make a conservative reconstruction.

5. **Professional edge quality**
   - All important boundaries should be crisp at 400–800% zoom.
   - Curves should have smooth tangent transitions and no obvious kinks.
   - Strokes should be consistent and joins/caps intentional.
   - Keep overlapping paths from creating dark doubled borders or visible seams.
   - Use clipping masks only where they simplify construction and remain easy to edit.

## Workflow

### Step 1 — Inspect the source

- Locate the user-provided image(s) and inspect pixel dimensions, aspect ratio, and resolution.
- View the whole figure and zoom into dense or problematic regions.
- Identify panels, headings, labels, scientific objects, arrows, borders, and background.
- Build a short internal inventory of objects and their approximate bounding boxes.
- If several references are provided, identify which is the target image and which are close-up detail references.
- For a multi-image task, create a separate SVG artboard for each source unless the user asks for a combined layout.

### Step 2 — Choose the reconstruction strategy

Use a **hybrid, object-first approach**:
- Use manually authored SVG paths for irregular organic shapes and smooth ridge/area curves.
- Use native SVG shapes for circles, ellipses, rounded rectangles, lines, arrows, and simple geometry.
- Use live text for labels when the font can be approximated reliably.
- For highly distinctive lettering that must visually match exactly, consider a separate outlined-text version only if useful; keep a live-text version too when practical.
- Use gradients for diffuse injury spots, soft organ shading, and dimensional fills.
- Do not use automated tracing as the sole method. If a trace tool helps as a starting point, simplify and rebuild the resulting paths before delivery.

### Step 3 — Establish coordinate system and style

- Use the source image's aspect ratio and a `viewBox` that maps cleanly to the intended canvas.
- Work in a consistent coordinate system. Preserve proportional scaling.
- Sample the source palette and define reusable SVG gradients and styles.
- Establish a small set of stroke widths for outlines, inner detail, dividers, and arrows.
- Use a readable sans-serif font stack and preserve hierarchy: panel title, object label, annotation, footnote.
- Keep text positions stable and avoid collisions with paths or panel borders.

### Step 4 — Rebuild the figure by semantic objects

Construct the figure from back to front:
1. Background and panel borders
2. Large regions and organ/cell silhouettes
3. Internal membranes, folds, nuclei, cristae, lobes, or other meaningful anatomy
4. Proteins, vesicles, labels, and modification badges
5. Arrows, connectors, and timeline/plot elements
6. Text labels and captions

For every object:
- Create a coherent shape with a clear visual boundary.
- Keep outer silhouette and meaningful internal structures in the same named parent group.
- Use a small number of curves. As a practical guideline, a simple protein blob often needs about 8–18 smooth Bézier segments; a basic mitochondrion usually needs one outer path, one inner path, and a limited set of cristae paths. These are guidelines, not hard limits.
- Do not model soft gradients as stacks of dozens of closed shapes.
- For a ridgeline / area chart, make each ridge a single smooth top contour plus a simple closed fill path, with the outline separate only if needed. Use only enough control points to express the trajectory.

### Step 5 — Preserve scientific content

- Compare every label and symbol with the source.
- Preserve arrows' direction and endpoints.
- Keep modification badges (e.g. PTM labels), protein names, residue positions, plus/minus signs, up/down arrows, and Greek characters accurate.
- Check the relative positioning of labels to their associated objects.
- Keep figure panel order and reading flow unchanged unless asked to redesign them.

### Step 6 — Review at multiple scales

Perform a structured quality check:

**Whole-figure check**
- Correct aspect ratio and no unintended crop
- Same panel layout and reading order
- Consistent spacing, alignment, font hierarchy, and palette
- No missing labels, arrows, or objects

**Zoom check (at least 400%; use 600–800% for fine details)**
- No jagged boundaries, spikes, abrupt joins, open contours that should be closed, or awkward overlaps
- No clusters of redundant anchors
- No tiny stray path fragments
- No double strokes or seams between adjoining fills
- Small biological details look intentional rather than generic or malformed

**Editability check**
- Open/parse the SVG and verify valid XML
- Confirm objects have meaningful IDs and are grouped semantically
- Confirm labels are live text where possible
- Confirm no visible embedded raster is carrying the final illustration
- Check gradients, masks, and markers render correctly
- If possible, open/render the SVG in a second renderer or export a PDF to catch compatibility issues

**Text check**
- Verify all scientific words, capitalization, punctuation, subscripts/superscripts, and symbols.
- Do not rely solely on OCR. Manually compare the text in the final render to the source.

### Step 7 — Make targeted revisions without collateral changes

If the user points out local defects:
- Use the supplied close-up as a geometry reference.
- Identify the exact object/group responsible.
- Replace or revise only that object and its necessary adjoining paths.
- Preserve all unrelated content, labels, positions, and styles.
- Re-render the whole figure after the local replacement to check for new seams or alignment problems.
- If the user says “其他地方不变” / “keep everything else unchanged,” do not regenerate or redesign the full composition unnecessarily.

### Step 8 — Export and package

Provide, by default:
1. `*_editable.svg` — the main editable vector master
2. `*_vector.pdf` — vector PDF export when supported
3. `*_preview.png` — preview only
4. `README.txt` — brief editing instructions
5. `*_vector_package.zip` — convenient bundle

For multi-figure jobs, provide one SVG and one PDF per source plus a ZIP containing all outputs.

Before linking a file, verify the exact file exists at the stated path. Never invent download paths.

## Technical SVG guidance

- Use semantic IDs such as `damage-sensing-ridge`, `mitochondrion-cristae`, `label-k286la`, and `arrow-import`.
- Use `<title>` and `<desc>` at the root for accessibility and context.
- Prefer `<path>`, `<circle>`, `<ellipse>`, `<rect>`, `<line>`, `<text>`, `<linearGradient>`, and `<radialGradient>` as appropriate.
- Use `stroke-linecap="round"` and `stroke-linejoin="round"` for biological curves when consistent with the source.
- Use gradient definitions once in `<defs>` and reference them by ID.
- Use marker definitions for reusable arrowheads.
- Avoid excessive filters, blur effects, masks, and clipping paths that may render inconsistently in vector editors.
- Keep SVG valid XML. Escape `&` as `&amp;` in text and attributes.
- Use unique IDs. Do not reuse IDs for unrelated objects.
- Avoid raster `<image>` elements in the final artwork unless the user explicitly asks to preserve a particular embedded raster component.

## Output acceptance checklist

Do not declare the task complete until the following are true:

- [ ] Main deliverable is a genuine editable SVG.
- [ ] PDF is vector-based when included.
- [ ] Figure composition and scientific content match the reference.
- [ ] Important outlines are smooth and crisp at high zoom.
- [ ] No auto-trace debris or unnecessary path stacks remain.
- [ ] Paths use a restrained number of Bézier segments.
- [ ] Every small meaningful element is grouped and named.
- [ ] Text remains editable wherever practical.
- [ ] All labels, arrows, symbols, and panel order have been checked.
- [ ] Files exist and package opens successfully.
- [ ] Any unavoidable approximation (e.g. font substitution or subtle texture simplification) is disclosed honestly.

## User preference distilled from prior accepted examples

The user prefers:
- close visual fidelity to the reference;
- smooth, clear boundaries;
- low anchor/segment counts;
- one meaningful element per group;
- text editable as text;
- subtle gradients instead of many layered contour paths;
- local detail fixes without changing the rest of the figure;
- SVG as the editing master and vector PDF as a companion.

Do not claim pixel-perfect 1:1 fidelity unless it has actually been verified. Prefer saying “faithful vector reconstruction” and disclose any approximated texture or font differences.
