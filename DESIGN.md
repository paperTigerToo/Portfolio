# Portfolio — Design

The authoritative design for this site. Vocabulary is defined in [GLOSSARY.md](GLOSSARY.md); use those terms.
[docs/reference/yinger-style.md](docs/reference/yinger-style.md) is the **Reference** this design borrows from. It is source material, not the spec. Where the two disagree, this file wins.

**Status:** round 01 settled the material (matte clay). Round 02 settled the motion (lively and crowded). Round 03 settled the layout (quiet). Design signed off on 2026-11-12; `mockups/round-04/final.html` is the visual target. Since 2026-11-25 the live site is `site/` (the signed-off page with the Project List split into `site/projects.js`), published to GitHub Pages from the `Portfolio` repository. The Vite + TypeScript rewrite has not started. Open: line height, real content, custom domain.

## Concept

A single dark screen. In the middle, a **Field** of soft bone-white **Blobs** drifts like thick liquid; each Blob is a **Project** and clicking it opens that Project. Text sits in the four corners like instrument readouts. Adding work to the site means adding one entry to the **Project List**, nothing else.

## Principles

1. **Two colours do the work.** Near-black and bone-white. Rose appears only as light on Blobs, never on text or chrome.
2. **The Field is the imagery.** No photos, icons, illustrations or screenshots.
3. **Flat chrome, deep centre.** No shadows, borders or elevation on UI. All depth comes from the Blobs.
4. **Compact.** 4px rhythm, 12px edge padding, text pushed to the corners, no centred column, no max-width.
5. **One weight.** 400 everywhere; contrast comes from typeface and size.
6. **Every Project is reachable without the Field.** The Index always exists.

## The Field

| Aspect | Decision |
|---|---|
| Slots | Fixed count: 7 on desktop, 5 when the viewport is narrower than 700px. |
| Filling Slots | The Project List is dealt into Slots in order and repeats when it runs out. One Project fills every Slot. |
| Seed | Derived from the Project and its place in the Stream. Fixes shape, size and drift character, so the site looks the same on every visit and repeats of one Project look different. |
| Bodies | Separate soft bodies. They squish flat against each other with a visible seam and **never fuse**. |
| Depth | Shallow layer. Blobs move in the screen plane with slight depth wobble; none goes far behind another. |
| Idle | Lively drift and strong surface wobble. With `prefers-reduced-motion`, no drift and near-zero wobble; Blobs respond only to input. |
| Motion | Lively and crowded (round 02, variant D). Blobs are large enough to fill about half the Field, so most are pressed against a neighbour at any moment. Collisions are fairly stiff with a little bounce; a grabbed Blob follows the pointer closely. Reference values: the `combo` preset in `mockups/round-04/final.html`. |
| Appearance | All Blobs are bone-white and differ only in shape and size. No per-Project colour, icon or image. |
| Light | One warm key light from the upper left; rose rim light from the lower right. The rose rim brightens on the hovered or selected Blob. |
| Material | Matte clay: soft diffuse bone-white, no sharp highlights or reflections, darkening in the seams where Blobs press together. |

## Interaction

| Input | Effect |
|---|---|
| Wheel, trackpad, drag or swipe on empty space, arrow keys, Page Up/Down, Space | Drives the **Stream**: vertical, endless, wrapping, in Project List order. Blobs leave one edge and enter the other carrying the next Projects. The page itself never scrolls. |
| Pointer passing over Blobs (mouse) | Nudges them aside. |
| Press and move on a Blob | Grabs it; release flings it into the others. Never opens a link. |
| Press without moving on a Blob (mouse) | Opens the Project in a new tab. |
| Hover (mouse) | Shows the **Label**: Project number, title, blurb, tags. |
| Tap a Blob (touch) | First tap selects it and shows the Label with an "Open" pill. Second tap, or the pill, opens the Project. Tapping empty space deselects. |
| Index pill | Shows the **Index** over the Field. |

## Layout

Quiet (round 03, variant C). Full-bleed, 12px from each edge. Text is kept to thin strips at the top and bottom so the Field has the screen. There is no hero clock.

| Place | Content |
|---|---|
| Top-left | Wordmark (owner's name), display face, links home. |
| Top-right | Index pill. |
| Bottom-left | One-line bio, and under it a single telemetry line: local time, location, role. |
| Bottom-right | Social pills in one row, text only. On screens narrower than 700px the bottom-left block sits above this row. |

The **Label is docked**, not floating: when a Blob is hovered or selected, its Label replaces the bio in the bottom-left, and the bio returns when nothing is hovered.

**Light falloff keeps text readable.** The key light fades out behind the top and bottom text strips: a Blob passing behind text dims to about a sixth of its brightness, over a soft gradient, so bone-white text always sits on something dark. The bottom falloff rises when the Label is showing, because the Label is taller than the bio. Text is drawn normally, with no blending tricks.

**Open — line height:** the Reference's 1.25 ceiling is in use; the owner has not confirmed it for multi-line text (bio, blurbs).

Real name, bio, location and social links are **not yet supplied**; mockups use placeholders.

## Tokens

| Token | Value | Use |
|---|---|---|
| `--carbon` | `#12130f` | Background, everywhere. |
| `--bone` | `#e4dfda` | All text; Blob surface. |
| `--vein` | `#3c3c38` | Focus and hover outlines on pills only. |
| `--rose` | `#f5c2c8` | Blob rim light only. |

Never pure black or pure white.

| Role | Face | Size / line-height | Notes |
|---|---|---|---|
| Display (wordmark) | Departure Mono (mockups use VT323 as a stand-in) | 40px / 0.7 | Blocky, digital. |
| Telemetry (labels, pills) | JetBrains Mono | 12px / 1.25, letter-spacing -0.6px, uppercase | All meta-information. |
| Body (bio, blurbs) | Inter | 16px / 1.25 | |
| Subheading (Label title, Index entries) | Inter | 30px / 1.13 | |

Spacing: 4px element gap, 12px edge padding, 64px section gap. Radius: pills 9999px, inline links 2px, nothing else rounded. Line-height never above 1.25.

The Reference's fonts (Arbeit, Inline VF) are commercial and are not used.

## The Project List

One file holding an ordered array. Each entry: `title`, `url`, and optional `tags`, `blurb`. Projects carry no dates. File order is Stream order; put the newest first by hand. The Field, the Label and the Index are all generated from it.

## Technical shape

- Static site: Vite + TypeScript, no UI framework, deployed to GitHub Pages by a GitHub Action.
- The Field is one full-screen raymarched fragment shader (three.js), with Blob positions driven by a small physics simulation on the CPU. Contact flattening is done by passing each Blob's nearest contact planes to the shader.
- Slow devices: render resolution steps down automatically to hold frame rate, deciding within the first second, and climbs back when frames are fast. If WebGL is unavailable, the Index is shown alone.
- Startup: the shader compiles before the first frame (in parallel where the browser allows), the physics runs 1.5 s ahead so Blobs start settled, and the Field fades in over 0.9 s once frames are steady. Over that, the **loading screen**: one breathing, slowly changing-shape bone-white blob in the centre with a `Loading 42%` telemetry line, on the empty Field with the corner text already showing. It shows real progress (page, type, renderer, shader, settling), stays at least 1.4 s, then swells and fades as the Field fades in. It is plain HTML and CSS plus a small classic script, so it appears before three.js downloads. It gives up after 12 s or on any error so nobody is trapped. With reduced motion the blob is still.
- Shader cost: each pixel first marks which Blobs' bounding cylinders cover it (rays are orthographic), then marches only from the top of the highest bounding sphere, skipping any Blob whose bound is farther than the best hit so far. Starting resolution is capped at about 1.3 million pixels; quality then adapts from measured frame times. `window.__field.bench(q)` measures GPU time per frame at quality `q`.
- The Index is real links in the page, readable by screen readers and search engines.
- Hosting: GitHub Pages from the `Portfolio` repository; `.github/workflows/pages.yml` deploys the `site/` folder on every push to `main`.
- **Open:** custom domain. Nothing else depends on it.

## Mockups

Throwaway single-file sketches in [mockups/](mockups/), three.js from a CDN, no build step. They are a visual record and are never ported line-for-line; the real site is written fresh once a direction wins. Serve the folder over HTTP to view them (they do not run from `file://`).

| Round | Question | Variants | Outcome |
|---|---|---|---|
| 01 | Which material? Motion and layout identical. | A matte clay · B glossy wet · C translucent gel | **A, matte clay.** |
| 02 | How should the Blobs move? Matte clay, layout identical. | A honey (slow, heavy, deep squish) · B water balloons (lively, bouncy) · C crowded (round-01 motion, larger Blobs packed tight) · D lively and crowded (B's motion at nearly C's size, added after review) | **D, lively and crowded.** |
| 03 | Where does the text go, on phone and desktop? Blobs identical. | A corners (floating Label, hero clock) · B readout (Label docked in the bio's place, hero clock) · C quiet (Label docked, clock reduced to a telemetry line, socials in one row) | **C, quiet.** |
| 04 | Final candidate: all locked choices plus the text legibility fix. | `round-04/final.html` | **Signed off** as the target for the real site. |
| 05 | Which loading screen? Live site underneath; each shows real progress (page, type, renderer, shader, settling) for at least 1.4 s. | A readout (telemetry checklist over the empty Field) · B single blob (one breathing CSS blob that swells away) · C curtain (bone panel with a big wordmark that lifts away) · D counter (giant percentage that fades away) | **B, single blob.** |

## Decisions

Newest last. Record what was chosen, what was rejected, and why.

- **2026-11-12 — Blobs never fuse.** Chose separate soft bodies that squish over lava-lamp metaballs that merge. Merging makes it unclear which Project a click lands on, and distinct bodies were the look the owner wanted.
- **2026-11-12 — Fixed Slot count, Projects repeat.** The Field should look equally full with one Project or thirty, so Blob count is constant and the Stream brings further Projects into view. Rejected: one Blob per Project with a cap.
- **2026-11-12 — Identity by shape only.** Kept the Reference's two-colour rule; rejected per-Project colours and thumbnails.
- **2026-11-12 — External links only.** A Blob opens the Project's own URL in a new tab; no detail pages on this site.
- **2026-11-12 — Kept the clock, dropped the FPS readout** from the Reference.
- **2026-11-12 — Corner text uses difference blending.** Found in round 01: bone-white text was unreadable when a bone-white Blob drifted behind it.
- **2026-11-12 — Material is matte clay** (round 01, variant A). Rejected glossy wet and translucent gel.
- **2026-11-12 — Motion is lively and crowded** (round 02, variant D). A blend of water balloons' speed and wobble with nearly the crowded size, with bounce reduced slightly so tightly packed Blobs don't jitter. Honey was not chosen.
- **2026-11-12 — Layout is quiet, with a docked Label** (round 03, variant C). Dropped the hero clock; time is now part of a telemetry line. Rejected the floating Label and the big-clock layouts, which gave the bottom third of a phone screen to text sitting on top of the Blobs.
- **2026-11-12 — Light falloff behind text, not difference blending** (round 04). Inverting text failed over a Blob's grey mid-tones. Dimming the Blobs behind the text strips works at every tone and reads as the Blobs leaving the light. Supersedes the round 01 difference-blending decision. Rejected: keeping Blobs out of the text strips, because the Stream has to carry Blobs through the top and bottom edges.
- **2026-11-12 — Owner signed off the round 04 final candidate** as the target the real site must match.
- **2026-11-25 — No dates on Projects.** Owner removed the year from the Project List, the Label and the Index.
- **2026-11-25 — Fade-in, not a loading screen.** Owner reported a laggy first load and first motion. Causes: a heavy first frame (shader compile) and a quality step-down too slow to catch up (about 3.5 s per step at 80 ms frames). Fixed by compiling ahead, removing unused material code from the shader, skipping Blobs too far away to matter, faster quality decisions and a fade-in. A full loading screen was rejected because the corner text can show at once.
- **2026-11-25 — Shader cost cut about 4.7x** (71 ms to 15 ms per full-resolution frame on the same scene) after the owner reported the Field was still choppy. Per-pixel candidate mask, bounded march start and end, pixel budget at start, faster quality step-down. The owner then asked for a loading screen as well (round 05).
- **2026-11-25 — Loading screen is the single blob** (round 05, variant B). Rejected the readout checklist, the bone curtain with a big wordmark, and the giant counter.
- **2026-11-25 — Published the signed-off page as the live site** (`site/`) on GitHub Pages, ahead of the Vite + TypeScript rewrite, so the site is online now. The rewrite replaces `site/` when it lands.

## For future agents

- Read [GLOSSARY.md](GLOSSARY.md) first and use its terms.
- Change this file when a decision changes; add a line to Decisions saying why.
- After each mockup round, fill in the Outcome column and move anything settled out of "Open".
- Do not copy the Reference's wordmark, copy or cube artwork.
