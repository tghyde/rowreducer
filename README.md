# Row Reducer

An interactive row-reduction practice tool for introductory linear algebra,
styled to match [hwGenie](https://github.com/tghyde/hwgenie) course sites.

**Live:** https://tghyde.github.io/rowreducer/

## Features

- Start from matrix dimensions or from a linear system (equations × variables,
  which builds the augmented matrix [A | b]).
- Entries accept integers (`-3`), fractions (`1/2`), and decimals (`0.25`);
  all arithmetic is **exact** (BigInt rationals — no floating point, ever).
- Click the gap between two columns to toggle an augmentation line at any
  column boundary (so [A | I] for inversion works too).
- Edit mode uses input boxes; reduce mode renders the matrix with KaTeX.
- Three row operations — Scale (Rᵢ → c·Rᵢ, c ≠ 0), Swap (Rᵢ ↔ Rⱼ), and
  Replace (Rᵢ → Rᵢ + c·Rⱼ) — with live previews, plus Undo and Restart.
- A badge appears when the matrix reaches REF or RREF. Convention: REF
  requires zero rows at the bottom and each leading entry strictly to the
  right of the one above (leading entries need **not** be 1); RREF
  additionally requires each pivot to be 1 and alone in its column.
- Full visual history of every matrix and operation.
- The URL hash always encodes the current reduction, so copying the link
  (or the **Copy link** button) reproduces the whole calculation.

## Implementation notes

Everything is one dependency-free `index.html` (KaTeX 0.16.21 from CDN is the
only external resource), deployed straight to GitHub Pages — no build step.

Matrix entries implement a small value interface (`add`, `mul`, `neg`,
`isZero`, `isOne`, `tex`, `toString`) with parsing isolated in
`parseEntry()`. Adding symbolic entries (variables) later means writing one
new value class and extending the parser; the row-operation, REF/RREF, and
rendering code is already generic over that interface. Share links store
entries as strings for the same reason.

Share-link format: `#` + base64url of JSON
`{v: 1, r, c, b: [bar boundaries], m: [[entry strings]], o: [ops]}` where ops
are `["s", i, "c"]` (scale), `["w", i, j]` (swap), `["c", i, j, "c"]`
(combine), rows 0-indexed, scalars as fraction strings.

The color theme, fonts, and flat card styling are copied from hwGenie's
`slate` theme, and the light/dark toggle shares the `hwg-theme`
localStorage key with the course sites.
