# MOMAR — Plan the first Marswalk, and explore the lunar south pole
VIDEO :
https://youtu.be/NEsPdoKwzB8?si=hqssM21Iwn1xqe6g
**MOMAR** (Moon + Mars) is a prototype for NASA Space Apps 2026, challenge *Interplanetary Survival Guide: Martian Map*. It puts real data from many NASA, ESA and USGS missions in one place, so an explorer can answer three questions anywhere on Mars:

1. **Where should I go?** Orbital imagery from 25 cm to 2.6 km per pixel, the USGS geologic map, CRISM mineral signals, thermal inertia, named features, Perseverance's and Curiosity's real traverses and Perseverance's photographs.
2. **How can I get there safely?** Slopes, a risk grid, and a **shortest** vs **lower-risk** Marswalk from A to B through up to 8 stops, with a return-to-base check.
3. **What science can I do on the way?** Stops where minerals show, mapped geology changes, or the ground turns to rock, each with its evidence and a confidence level.

**MOMAR Lunar** does the same for the Moon: a whole-Moon globe with every located lander and rover, and a south-pole planner laid out like the Mars one, on real LOLA terrain where the Sun and Earth stand where JPL Horizons puts them, terrain shadows and Earth line of sight are computed from the DEMs, Diviner temperatures and LOLA long-term sunlight show where ice could survive and where power and communications are best, routes are planned for an Apollo LRV-class rover, a VIPER-class rover or a walking astronaut, and the traverse plays in 3D with the Apollo Lunar Roving Vehicle and an astronaut walking in lunar gravity ([below](#momar-lunar-the-moon)).

**SOZO** (Scientific Observation Zone Operator), MOMAR's AI scientist from [sozo.momar.si](https://sozo.momar.si), is built into both planners: ask it about the plan on your map and it answers with MOMAR's own figures, a route comparison and cited research, while its 3D companion floats over the map ([below](#sozo-momars-ai-scientist)).

Every layer shows its mission, instrument, resolution, date, licence and status (real, sampled, streamed, derived, illustrative, missing). Nothing archived is shown as live, and nothing illustrative is shown as observed.

### MOMAR in numbers

<!-- numbers:start -->
Generated from the data manifests and the lunar registry by `npm run numbers`; a test fails if this block drifts (the landing page reads the same files):

| | Area | Primary terrain grid | Note |
|---|---|---|---|
| Jezero, Mars | 916 km² | 2,413,152 cells at 20 m | CTX DTM terrain |
| Gale, Mars | 493 km² | 1,237,488 cells at 20 m | 1 m MSL merged DEM sampled onto the 20 m grid |
| Detailed Mars regions | ≈ 1,410 km² | 3,650,640 cells | Jezero + Gale only, not the whole planet |
| Lunar south-pole workspace | ≈ 133,000 km² | 2,310,400 cells at 240 m | the square polar-stereographic grid (to ≈ 84°S), not equally observed everywhere |
| **Total** | | **≈ 5.96 million cells** | different resolutions; the Moon's 960 m far-field and its 480 m Diviner and evidence grids overlap the workspace and are not counted |

- **Names and objects:** 219 IAU-named features and the Perseverance landing site on the Mars globe; 2,045 IAU name points on the Moon (2,023 primary features plus satellite craters in the south); 71 located human-made objects on the Moon (46 pieces of hardware, 20 impact sites, 5 failed landings), counted separately from the names.
- **Data sources:** 16 available entries in the Jezero manifest and 12 in Gale's (per-region entries; the same product can serve both); the lunar registry has 25 integrated or MOMAR-derived entries and 4 listed as not integrated (M³, GRAIL, SLDEM2015, shape-from-shading 5 m DEMs).
- **Reference data:** SHARAD ground tracks: 49 over Jezero, 12 over Gale. MCS profiles summarised: 2,182 near Jezero, 2,191 near Gale. HiRISE DTMs at Gale, kept out of route risk: 11, on 82 % of its cells. CRaTER: 1,040 complete days, 2009-06-26 to 2012-12-31.
- **Kinds of data.** Mars: elevation models, orbital imagery, geology, CRISM mineral signals, THEMIS and TES thermal inertia, HiRISE 2 m slopes, SHARAD radar tracks, archived MCS climate, rover traverses, waypoints and photographs, archived MEDA weather, IAU names, and derived slopes, risk, routes, sunlight and shadows. Moon: LOLA elevation with its uncertainty, effective resolution and return counts, LROC NAC and ShadowCam imagery, permanently shadowed regions, roughness, Diviner temperatures and ice stability, LOLA long-term Sun and Earth visibility, the USGS ice favorability index, LEND neutron suppression, Mini-RF radar CPR, LAMP far-UV albedo, the CRaTER radiation reference, Kaguya mineral maps, global geology, IAU names, located human-made objects, and computed Sun and Earth positions, terrain shadows and routes. Ice layers show potential or stability, never confirmed ice.
<!-- numbers:end -->

---

## Contents

- [MOMAR in numbers](#momar-in-numbers)
- [What you can do](#what-you-can-do)
- [Quick start](#quick-start)
- [Tour of the app](#tour-of-the-app)
  - [Landing page and planet picker](#landing-page-and-planet-picker)
  - [Whole Mars globe](#whole-mars-globe)
  - [Plan anywhere on Mars](#plan-anywhere-on-mars-globe-sites)
  - [The planner (Jezero and globe sites)](#the-planner-jezero-and-globe-sites)
  - [3D mission playback](#3d-mission-playback)
  - [SOZO Chat and the Mission Scientist](#sozo-chat-and-the-mission-scientist)
- [SOZO, MOMAR's AI scientist](#sozo-momars-ai-scientist)
- [MOMAR Lunar (the Moon)](#momar-lunar-the-moon)
- [How it works](#how-it-works)
- [Data](#data)
- [Real, sampled, assumed, illustrative](#real-sampled-assumed-illustrative)
- [Regenerating the data](#regenerating-the-data)
- [Third-party 3D models (audit)](#third-party-3d-models-audit)
- [Verification](#verification)
- [Limitations](#limitations)
- [What to improve next](#what-to-improve-next)
- [Repository map](#repository-map)
- [Credits & licences](#credits--licences)

---

## What you can do

| | Feature | Where |
|---|---|---|
| 🌍 | **Fly over the whole planet**, sharp from orbit down to ~3 km up: real Viking colour, then the 5 m CTX mosaic streamed as you zoom; MOLA relief in profile (true scale, 3× or 10×, labelled); 219 IAU-named features and the Perseverance landing site, with search, hover details and live coordinates | `/globe/` |
| 📍 | **Land anywhere** between 80°S and 80°N and get the **full planner** on that spot: 200 m HRSC + MOLA terrain, CTX imagery, dust / sand / rock classes from night-time infrared, suggested routes from the site's own data | `/globe/site/?at=lat,lon` |
| 🛰️ | **Jezero Crater in depth**: 20 m terrain, 6 m and 25 cm imagery, geologic map, CRISM minerals, thermal inertia, Perseverance's traverse and 626 raw images, archived weather | `/planner/` |
| 🏔️ | **Gale Crater in depth**: the 1 m MSL merged DEM sampled to 20 m, the CTX mosaic, CRISM minerals, thermal inertia and Curiosity's 38 km traverse, with scenarios from Curiosity's own stops up Mount Sharp | `/planner/?region=gale` |
| 🌡️ | **Thermal inertia**: THEMIS (100 m) and TES (3 km) maps and readouts, measured-derived and labelled with their uncertainty, beside the night-IR surface proxy; TES at every globe site | Layers |
| 🧭 | **Plan a Marswalk**: A → S1…S8 → B, shortest vs lower-risk routes found by any-angle search (with the A\* engine as a checked fallback), an explained comparison, hazards, elevation profile, return-to-base and EVA time checks, editable assumptions | planner |
| ☀️ | **Mars time and sunlight**: pick the sol and local time; see the Sun's position (Mars24), sunrise and sunset, terrain shadows on the map, and how much of the walk is in sunlight | planner |
| 🔬 | **Science stops** with evidence: CRISM absorptions, mapped geologic contacts, or (at globe sites) where the route reaches night-warm rocky ground; the CRISM observations covering any stop, from the PDS archive on demand | planner |
| 🚙 | **Drive it in 3D** with NASA's **Perseverance** or **Spirit & Opportunity** model, or an animated astronaut: rolling wheels that each stay on the ground, wheel tracks, gravel, platy slabs, bedrock pavement, layered outcrops, dust and dune sand | 3D view |
| 🌪️ | **Martian weather** (illustrative): clear, dusty with dust devils, or a dust storm with a dim ochre sky and blowing dust | 3D view |
| 🌏 | **The sky over the rover on any date**: the Sun moves with the walk, night falls after sunset, and Earth stands where the planetary ephemeris puts it; Look at → Sun or Earth (Mars, 1976–2050; Moon, 1966–2045) | 3D view |
| 🤖 | **Ask SOZO, MOMAR's AI scientist**, about the plan on your map: answers from MOMAR's figures, a route comparison, cited research and its limits; paste your SOZO key in Settings and SOZO appears in 3D (drag it anywhere, click to chat), also inside the 3D playback. Or ask Claude, OpenAI or a model on your own computer (Ollama, LM Studio), every number checked against MOMAR's data, or the built-in offline Mission Scientist | SOZO Chat, `/sozo/` |
| 🛰️ | **Orbital evidence and reference data**: on the Moon LEND (potential hydrogen, never confirmed ice), Mini-RF CPR and LAMP far-UV albedo as layers, and a CRaTER radiation reference; on Mars SHARAD tracks with archive links, an archived MCS climate panel, and HiRISE 2 m slopes at Gale. All labelled as references: they change no route or risk score | Layers, Conditions |
| 🖨️ | **Export**: a printable mission plan with a map, and GeoJSON in Mars coordinates | Mission plan |
| 🌕 | **The whole Moon**: LROC WAC imagery, LOLA elevation anywhere, IAU names, geology and Kaguya composition, real sunlight for any time, and every located lander and rover (LROC) | `/moon/` |
| 🧭 | **Lunar south pole planner**: LOLA terrain (240 m, 5 m within 87°S), LROC NAC and ShadowCam 1 m imagery, permanently shadowed regions, Diviner cold traps and ice stability, LOLA long-term Sun and Earth visibility, computed sunlight and shadows, the Earth in the sky; A → S1…S8 → B routes for an Apollo LRV-class rover, a VIPER-class rover or a walking astronaut (cited values), science along the way, site comparison, LROC images of any point | `/moon/south-pole/` |
| 🚙 | **Drive it on the Moon in 3D** (full-screen mission playback, laid out like the Mars one): the Apollo LRV (Deutsches Museum model) with rolling, steering wheels, wheel tracks and dust flying ballistic arcs in vacuum, the crew walking out at each stop, or an astronaut loping in lunar gravity; 5 m LOLA terrain with illustrative small craters and rocks close in, GPU shadows from the computed Sun; the Sun and Earth where they really are; a mission inspector with stops, profile and what is at each position | 3D view |
| 🛰️ | **Lunar mission catalogue**: hardware on the surface, unsuccessful landings, impacts and planned vehicles, each with its source and accuracy | `/moon/missions/` |

---

## Quick start

```bash
npm install
npm run dev          # http://localhost:3000
```

The processed Jezero data (~20 MB) and the globe base maps (~3.5 MB) are committed in `public/data/`, so the app runs without the Python pipeline. Globe sites and fine imagery are read live from public archives (no keys).

| Command | What it does |
|---|---|
| `npm run dev` | Development server |
| `npm run build` / `npm start` | Production build (every page prerendered, plus the four `/api/sozo/*` server routes) and serve it |
| `npm test` | Unit tests (Vitest): SOZO context mapping, proxy security, short answers and when SOZO surveys the map; Mars terrain, any-angle routing and route smoothing, Mars time and Sun position (Mars24), terrain shadows, Earth in the Mars sky, globe relief, CRISM on demand, walking-time models, validation against Perseverance's drives, science, comparison, globe geometry, GeoTIFF reading, surface classes, rocks, playback, rover models and wheel rigs, asset rules; Moon frames and projections, Sun/Earth against JPL Horizons, horizons, illumination, COG reading, routing, shadows, ranking, catalogue, chat engines (Claude, OpenAI and local models, mocked), report, traverse timeline, lunar gait, LRV rig, mobility presets, science along routes, Diviner and LOLA visibility products, ODE footprints, illustrative craters and rocks, ballistic dust |
| `npm run typecheck` / `npm run lint` | TypeScript and ESLint |
| `npm run data` | Re-run the Jezero data pipeline (see [Regenerating the data](#regenerating-the-data)) |
| `.venv/bin/python scripts/build_moon.py [steps]` | Re-build the lunar data (see [MOMAR Lunar](#momar-lunar-the-moon)) |

**Deploy:** push to Vercel (it builds Next.js with its own builder). Every page is prerendered; the only server code is the SOZO proxy (`src/app/api/sozo/`), which runs as Vercel functions so SOZO keys never reach the page's scripts. **No environment variable is needed**: visitors connect SOZO by pasting their own key in SOZO Chat → Settings; until then the built-in Mission Scientist answers. Optional host settings (`.env.example`): `SOZO_API_KEY` (a key for visitors who have none), `MOMAR_SESSION_SECRET` (also encrypts saved keys), `SOZO_BASE_URL`. A plain static host can serve the pages, but not SOZO; Claude, OpenAI and local models work there too, since they are called from the browser.

**Contact form:** opens a Gmail compose window addressed to partnerships@siwom.in with the visitor's name, topic and message filled in (a `mailto:` link is the alternative); nothing is sent from the site. Optional public build-time overrides (see `.env.example`): `NEXT_PUBLIC_CONTACT_EMAIL` changes the recipient; `NEXT_PUBLIC_CONTACT_FORM_ENDPOINT` posts the form to a JSON form service instead. It never fakes a successful submission.

---

## Tour of the app

### Landing page and planet picker

`src/app/(site)/page.tsx` is a cinematic Moon + Mars landing page, separate from the planner.

| Section | Notes |
|---|---|
| Navbar & brand | Logo `public/brand/momar-logo*.{png,webp}`; site icon cropped from the logo's moon. Plus Jakarta Sans. Links: What we build, Mission, Vision, How it works, About, **Company**. |
| Hero | Full-bleed background video `public/videos/landing page.mp4` (1280×720, 10 s, H.264), a **concept animation** labelled as such: muted, looped, inline, `object-cover` under a dark overlay. The poster `public/videos/landing-poster.webp` is the video's opening frame; it shows while the video loads and stays if it fails. A pause button is always available; under reduced motion the video waits for play. |
| What we build now | A **Mars / Moon switch** (Mars first) over the figures above, computed at build time from the Jezero, Gale, lunar and globe manifests (area, terrain cells, data sources, names), and six cards per world with real screenshots captured from the running app (`public/images/product/`: `planner-*.webp` for Mars, `moon-*.webp` for the Moon), the planner link, the sources and a link to their limits. |
| Moon → Mars | Second full-bleed **concept animation**, `public/videos/landing page 2.mp4` (1280×720, 10 s, H.264): a lunar launch base that becomes a Mars settlement. Labelled as concept; poster `public/videos/landing-2-poster.webp` (its opening frame). It loads nothing until the section nears the screen, plays only while visible, and has the same pause button and reduced-motion behaviour as the hero. |
| The Moon / Mars | Lazy-loaded Three.js viewers (`PlanetViewer.tsx`), only rendering while visible, static under reduced motion, static images without WebGL. |
| Robotics & Infrastructure | Five concept illustrations in `public/images/concept/robotics/` (planetary rovers, robotic manipulators, autonomous exploration, Lunar & Martian habitats, robotic construction; supplied artwork, WebP 1440 px), each badged *Concept art* under *Concepts · planned research*, with a note that none of these robots, vehicles or bases exists on Mars today. |
| Founder & recognition | Driven by `src/content/recognition.ts`: highlights (NASA Space Apps 2024 Local Award Winner and Global Nominee, 2025 Global Nominee, 3× ISRO Bharatiya Antariksh Hackathon), certificates with originals in a lightbox (an entry appears only if its file exists at build time), and the NASA and ISRO programme logos (`public/images/logos/SOURCE.md`). |
| Company page | `/company/` (`src/app/(site)/company/page.tsx`): **MOMAR Exploration Technologies**, the company behind MOMAR. A *MOMAR Exploration Technology* section with a linked preview of its site, [space.siwom.in](https://space.siwom.in/) (`public/images/company/siwom-space.webp`; the site can't be embedded, it forbids framing), the six-stage pipeline, capabilities with the site's own Available / Prototype / Research / Planned labels, projects (the MOMAR Terrain Lab, MOMAR), roadmap, research principles and what the company does not claim. Summarised from space.siwom.in, with the date read; web addresses are kept as they are. |
| Join | GitHub issue link plus the contact form. |

**Open Planner** (every page) leads to `/explore/`, a planet picker with 3D Moon and Mars globes (`PlanetPicker.tsx`). Choosing Mars offers **Jezero Crater** (`/planner/`) or **Whole Mars** (`/globe/`); choosing the Moon offers the **Lunar south pole** workspace (`/moon/south-pole/`) or the **Whole Moon** (`/moon/`), and the mission catalogue.

Planet models (fetched by `scripts/fetch_planet_assets.py`, recorded in `public/models/SOURCES.json`): **Mars** is NASA Science's *Planet Mars, 3D Model* (NASA/JPL-Caltech, GLB 4.0 MB); the **Moon** is NASA SVS's *CGI Moon Kit* (LRO LROC WAC colour and LOLA elevation on a sphere; NASA's packaged Moon GLBs are 13–300 MB, too heavy for a landing page).

### Whole Mars globe

`/globe/` — `src/components/globe/marsGlobe.ts`, `GlobeExplorer.tsx`, `src/lib/globe.ts`, `marsTiles.ts`

- **Sharp at every scale.** The surface is a quadtree of sphere patches refined by level of detail: a patch splits when one texel of its imagery would cover more than ~1.25 screen pixels. Each patch draws the best real imagery loaded for it:
  - whole-planet views: the build-time Viking MDIM 2.1 colour mosaic (8192 × 4096, ~2.6 km/px), shaded by MOLA relief (stretched ×6, labelled);
  - zooming in: 512 px tiles of the **MDIM 2.1 colour mosaic** (231 m/px; tiles to ~160 m/px) streamed from the public **OnMars** tile services;
  - closer still: the **Murray Lab CTX global mosaic** (~5 m/px at the deepest level), its brightness coloured by the MDIM tile above it. The blend is chosen per screen pixel by its ground size, so no patch edges show.
- **Relief in profile.** The globe's surface is displaced by **MOLA elevation** (`elevation-2k.png`, the 463 m DEM as 2048 × 1024 block means, ≈10 km/px, lossless 16-bit) times a labelled vertical exaggeration: **flat**, **true** scale, **3×** (default) or **10×**, so Olympus Mons, the Tharsis volcanoes and Valles Marineris stand out at the limb. Displacement is computed once per patch on the CPU from one global height field (`src/lib/globeRelief.ts`); picking, labels, the selection marker, the horizon test and the camera's floor use the same heights. Heights are areoid-referenced on a sphere (Mars's flattening is not shown).
- **No holes, no cracks, no seams.** A patch whose tile has not arrived draws its nearest loaded ancestor's tile, cropped; patches carry skirts, sunk by the patch's own height range (sampled along its edges between vertices), so displaced neighbours of different levels never open cracks (tested at 10×); the ±180° wrap is seamless. Tiles load coarse-first, six at a time, are cancelled when no longer wanted, and are released least-recently-used beyond ~220.
- **Always says what you see.** The header names the imagery on screen and its resolution ("Imagery: CTX mosaic · 41 m/px") and how many tiles are loading. If the tile service stops answering it says so, shows the 2.6 km/px base map and backs off (one probe at a time, 15 s pauses).
- **Places.** 219 IAU-named features (USGS Gazetteer) plus the Perseverance landing site, drawn as dots with labels, decluttered by importance (a hidden neighbour counts into the label in front as "+n"). Hover a label for its type, size, coordinates and source; the coordinates under the pointer are shown live. Search by name or use the quick list (Jezero, Gale, Olympus Mons, Valles Marineris, Hellas…); a search flies to about three diameters up.
- **Pick a point.** A click shows its coordinates, its **MOLA elevation read live** from the USGS 463 m mosaic (e.g. 20,012 m at the IAU centre of Olympus Mons), the most specific named feature containing it, and **Land a rover here →**. Inside the Jezero build area it also offers the detailed planner. The selection lives in the URL (`#ll=lat,lon`) for sharing.
- **Diagnostics** (`/globe/?debug` or the D key): landmarks loaded and visible, camera distance and altitude, selected lat/lon, base map status, patches drawn and finest level, the imagery shown, tile service status, tiles loaded / cached / pending / failed with the last failures, and patch outlines (hue = level, hatched = drawing an ancestor's imagery).

### Plan anywhere on Mars (globe sites)

`/globe/site/?at=lat,lon` — `src/lib/site.ts`, `siteRegion.ts`, `vocab.ts`, `src/components/planner/useRegion.ts`, `siteSource.ts`

**Land a rover here →** opens the **full planner** on that spot, built in the browser from the place's own data, read straight from the USGS Astrogeology archive on S3. These are uncompressed GeoTIFFs: one HTTP range request per row, cross-origin allowed, no key, no server, retried on network errors.

| What | Data | Notes |
|---|---|---|
| Terrain | **HRSC + MOLA blended DEM**, 200 m posts (HRSC stereo where it exists, MOLA elsewhere), a 96 × 96-post window (~19 km) | Falls back to **MOLA 463 m** (48 × 48 posts) if it cannot be read |
| Surface | **THEMIS night-time infrared** (100 m, 60°S–60°N) and the brightness of the **Viking colour mosaic** (≈ 160 m) | Classes below |
| Map imagery | **CTX global mosaic** (≈ 5 m) coloured by the Viking mosaic (CSS colour blending), THEMIS day IR and MOLA relief, streamed | If CTX does not load, the map says so and keeps MOLA relief |
| 3D ground | CTX stitched for the area shown, coloured by the Viking mosaic | |
| Names | IAU features in and near the site | |

**Surface classes**, relative within the site (median ± a robust spread of at least 6 DN):
- **fine dust**: night-cold ground (low thermal inertia);
- **rock, bedrock or crust**: ground that stays warm at night;
- **dark sand (dunes)**: night-warm *and* dark, as in Gale's Bagnold dunes;
- **mixed regolith**: everything else.

These are a proxy, not calibrated thermal inertia. Dust and dark sand count as loose ground in the route risk. Rocky ground gets more illustrative rocks and outcrops in 3D, and dust and dune sand are drawn as such. Mars has no liquid mud today: wet- or muddy-looking ground in orbital images is dust, sand or dark basaltic sand, and MOMAR says so rather than inventing mud.

**Suggested routes from the site's data** replace Jezero's rover scenarios, and the first one loads at once:
- to the largest patch of night-warm (rocky) ground;
- a lookout on the highest gentle ground;
- the lowest ground;
- a named feature inside the site;
- a survey: A → rocky ground → lookout.

**Science stops** mark where a route enters night-warm ground (the likeliest exposed rock), with low confidence and a stated reason.

Every label that names terrain data follows the site (`vocab.ts`): "200 m data only", "loose ground (night IR)", the slope baseline, the caveats. Panels that need Jezero-only data (geology, CRISM, HiRISE, weather records, rover photographs) say so. The planner is ready in about 14 s on a normal link (longer on a slow one). Old `#site=` links redirect here.

### The planner (Jezero and globe sites)

`/planner/` and `/globe/site/` — `src/components/planner/Planner.tsx` and panels

- **Side panel** (Plan, Images, Layers, Assumptions, Conditions): on wider screens it folds to a narrow rail of symbols (the arrow at the start of its tabs) to give the map the room; a symbol opens the panel on its tab, and the choice is remembered in this browser (`src/components/site/PanelRail.tsx`, also on the Moon). Phones keep the bottom sheet.
- **Regions**: Jezero (Perseverance) and Gale (Curiosity), chosen in the header; the same planner, layers and 3D on each region's own data, and every dataset a region lacks is listed with the reason (Gale has no detailed geologic map service, no HiRISE fine slopes, no weather records and no rover photographs in MOMAR).
- **Map** (Leaflet, Mars equirectangular CRS, R = 3396.19 km): imagery, derived terrain layers (elevation + hillshade, slope, live risk, data quality, HiRISE 2 m slope, surface classes, sunlight and terrain shadow), science layers (CRISM composites and footprints, geologic units, eolian bedforms), mission context (traverse, landing ellipse, named features). Every layer has an info card with provenance, derivation and legend; the Layers tab has search and **Restore defaults**. A hover readout gives coordinates, elevation, slope, unit or surface class and data quality.
- **Itinerary**: start A, up to 8 ordered stops, destination B. Place them by clicking the map or typing coordinates; drag, reorder or remove stops; add a science suggestion with its evidence. A stop on impassable terrain moves to the nearest walkable cell and says how far. Shared links carry the plan (`#a=lat,lon&s=lat,lon,contact;lat,lon&b=lat,lon`); a science stop's evidence is re-detected on the planned route, never read from the URL.
- **Routes**: *shortest* and *lower-risk*, leg by leg. Each leg runs an **any-angle search (Theta\*)** and the original A\* engine; the cheaper valid route under the route's own model is kept, and the panel says which, by how much, and the two lengths and amounts of turning (**Show the grid path on the map** overlays the A\* grid path for comparison), and an **explained comparison**: distance, walking time, climb, steepest cell, length steeper than 15°, loose ground, coarse-data length, risk score. Δ is computed from the shown values. Where the routes separate (more than 50 m apart for 200 m or more), each stretch is listed and highlighted on the map; when none qualifies, the text says the routes are effectively the same rather than claiming a benefit. Details give hazard stretches, the risk score split by term, risk bands, assumptions and caveats.
- **Elevation profile** with stops marked, linked to the map. **Return-to-base** check against an EVA time budget, including time at science stops, with a second, low-confidence walking-time estimate beside Tobler's (Minetti et al. 2002 gradient costs with the climbing part scaled to Mars gravity, `src/lib/walkModels.ts`).
- **Departure & sunlight**: pick the sol (Perseverance's mission sols in Jezero, Mars Sol Dates elsewhere) and the local true solar time; the panel shows UTC, the mission clock (LMST), Ls and season, the Sun's elevation and azimuth, sunrise and sunset, and how much of the walk out is in sunlight, in **terrain shadow** or with the Sun down, with the arrival time before or after sunset. **Sunlight & terrain shadow** is also a map layer for the departure time. Sun position by the Mars24 algorithm (`src/lib/marsTime.ts`), shadows cast by the DEM itself with Mars's curvature (`src/lib/marsShadow.ts`).
- **Thermal inertia**: THEMIS-derived (≈ 100 m, Fergason et al. 2006, ±20 %) and TES-derived (≈ 3 km, Putzig & Mellon 2007, infilled pixels flagged) as map layers with indicative classes (dust < 100, sand to ≈ 420, crusted or rock-rich ground, bedrock > 1200 TIU) and in the hover readout. Shown for reference: they do not change route risk. Straight-edged strips in the THEMIS layer are seams between images in the mosaic.
- **What the route is based on**: every route is split by the data under it (HiRISE-sampled, the region's 20 m model only, coarse or gap-filled), adding up to its length, in the route panel, the report and the GeoJSON. Detected obstacles: none. No georeferenced boulder or rock catalogue exists for Jezero or Gale in MOMAR, so features smaller than a few model cells are not avoided; the 3D rocks are illustrative and the routing code cannot reach them (a test walks its import graph).
- **CRISM observations here**: each itinerary stop can list the CRISM observations covering it (type, date, Ls, incidence, detectors, archive links), fetched on demand from the PDS Orbital Data Explorer (`src/lib/crismOde.ts`). Archive records only: MOMAR derives no spectra.
- **Assumptions** (all editable, labelled *planning assumptions, not NASA suit or EVA limits*): slope limit 25°, walking speed 2.5 km/h with Tobler's function, risk weights, risk aversion, EVA budget 4 h, 20 min per science stop.
- **Images** (Jezero): 626 curated Perseverance raw images with camera, sol and "near a stop" filters, and an inspector with caption, IDs, times, site/drive, pointing, credit and links. Images are placed at the rover's **localized position**, never at a planned stop, and the map says it shows *the rover's position, not the camera's view*.
- **Conditions** (Jezero): archived MEDA air temperature and pressure (24 sols across a Mars year) and a labelled Curiosity RAD radiation reference.
- **Mission plan report** (printable): a map snapshot (local CTX in Jezero, the CTX global mosaic at sites), itinerary with evidence and arrival times, legs, the comparison, profile, waypoints with ETA and heading, risk summary, data gaps, science stops, return check, assumptions, sources. **GeoJSON** export in Mars planetocentric coordinates.

### 3D mission playback

`Terrain3D.tsx`, `PlaybackPanel.tsx`, `terrainMesh.ts`, `rockLayer.ts`, `routeRibbon.ts`, `marsAtmosphere.ts`, `src/lib/playback.ts`, `walk.ts`, `rocks.ts`, `terrainDetail.ts`

**The ground**
- **One surface for everything.** The terrain's posts (20 m CTX DTM in Jezero, 200 m at globe sites) keep their exact heights. Between them is a smooth Catmull-Rom interpolation, plus a deterministic procedural **micro-relief** below what the data resolve: 2–16 m undulations and bedrock ridges and small degraded craters in Jezero; on coarse global terrain also 32–256 m relief and 20–120 m craters. The GPU terrain, wheels, feet, rocks, route bands, markers and camera all read this one function. Its GLSL twin matches the TypeScript to 5 µm. It is anchored to absolute coordinates, so a place always looks the same. **Illustrative detail: Off** shows the measured terrain alone.
- **GPU terrain**: nested grids centred on the view, 0.25 m spacing near the camera and doubling outwards, placed in the vertex shader. Levels morph into each other without cracks; moving costs a few uniforms, never a rebuild.
- **Ground detail** (illustrative, near the camera, guided by the data):
  - **gravel**: grey basalt and pale clasts 2–5 cm across, each with a contact shadow, mostly on rocky and mixed ground;
  - **fractured bedrock slabs**: pale plates split by sand-filled joints, on bedrock-like bare ground;
  - **layered beds** on steep faces (delta scarps, crater walls);
  - **rippled sand** on mapped bedforms or dune sand (dark where the data show dunes);
  - smooth, bright **fine dust** on night-cold ground.

  Colours were tuned against Viking lander views, then made a little less saturated (ground 20 %, rocks 25 %), towards the tan and grey-brown of Perseverance Mastcam-Z colour images.
- **Rocks** (illustrative, never hazards in the planner): sizes from the Golombek & Rapp (1997) rock size–frequency model, *F(D) = k·exp(−q(k)·D)* with *q(k) = 1.79 + 0.152/k*, at an assumed abundance *k* = 5 %, from 4 cm pebbles to 2 m boulders. Each sits on the full surface, tilted to it and partly buried. Density follows the data through an assumed relation: more where HiRISE roughness is higher (Jezero) or the ground is night-warm (sites), fewer on sand and dust. They lie under and around the rover; only rocks taller than 30 cm are kept out of the wheels' path and only those over 6 cm off the walking line. The rover rides up over the rocks it meets: each wheel rests where its rim first touches ground or rock.
- **Platy slabs**: about a third of rocks and boulders are flat, angular slabs (broken bedding planes, as in rover views of outcrop debris), lying tilted up to 18°.
- **Outcrops** (Normal and High): layered bedrock ledges 2.5–9 m long, built from 3–6 stacked irregular beds with pale and dark bands and dust on the treads, at an assumed 55 per km² and only where the data mark the ground as rocky; kept out of the route's path. Not part of the size model and not observed outcrops.
- **Detail levels**: High (default on desktops) draws rocks 45 % farther out and more outcrops; Normal (default on tablets), Low (default on phones) and Off are the others.

**The rovers** (choose in the 3D toolbar; `src/lib/roverModel.ts`)
- **Perseverance** (2021): NASA/JPL-Caltech's model at true scale (2.7 m wide, 2.2 m tall, 52.5 cm wheels), meshopt-compressed from 11.7 MB to 4.9 MB (`public/models/perseverance/SOURCE.md`).
- **Spirit & Opportunity** (2004): NASA/JPL-Caltech's model at true scale (2.28 m wide with the solar arrays, 1.58 m tall, 26 cm wheels), from 11.9 MB to 970 KB with meshopt and WebP textures (`public/models/mer/SOURCE.md`). Its wheels share a mesh with the suspension, so a triangle joins a wheel only if all its corners lie inside that wheel's cylinder; the rest of the suspension stays. Its top speed (5 cm/s) is used for its drive-time estimate.
- **Rolling wheels**: on both, the six wheels are split from the model at load and hung on their own axles, rolling by distance ÷ radius.
- **On the ground**: six wheel contacts are sampled on the drawn surface (each wheel's footprint, rocks included), a plane is fitted and the body sits exactly on it. Then each wheel drops onto the ground or rock under it within its suspension travel (±0.3 m, ±0.15 m for MER), as a rocker-bogie keeps all six down. Measured over 300 frames at 2× playback: Perseverance's wheels average 0.4 mm from the ground (worst 1.1 mm into it, 1.3 mm above it); Spirit & Opportunity's 2.6 mm (worst 4.6 mm into it); no frame had a wheel sunk more than 2 cm or lifted more than 3 cm. The rocker and bogie arms themselves stay rigid.
- Heading is smoothed over ±4 m so corners are driven as arcs, then eased so time-lapse never snaps it.
- **Wheel tracks** (illustrative, at each rover's own wheel spacing and width, with grouser marks) are pressed behind it as it drives, and dust puffs rise from its wheels.

**The astronaut**
- **Rover / Astronaut / Both**: "Animated Astronaut Character in Space Suit Loop" by [LasquetiSpice](https://sketchfab.com/LasquetiSpice) (CC BY 4.0), optimized from 46.6 MB to 3.2 MB.
- Its low-gravity lope is adapted to a walk with a foot always down, following published reduced-gravity and suited-walking studies ([Kram et al. 1997](https://doi.org/10.1242/jeb.200.4.821), [Carr & McGee 2009](https://doi.org/10.1371/journal.pone.0006614), [Cavagna et al. 2000](https://doi.org/10.1111/j.1469-7793.2000.00657.x), [Cullinane et al. 2017](https://doi.org/10.3357/AMHP.4650.2017)). The result is 0.50 m steps at 82 steps/min at the planner's 0.69 m/s.
- Phase is locked to distance, planted feet don't slide, two-bone leg IK fits feet to slopes, and the body leans gently into climbs.
- In Both mode the rover escorts 8 m behind.
- This is a visualization, not spacesuit biomechanics; no human has walked on Mars.

**Sun, time and Earth** (`marsTime.ts`, `marsSky.ts`)
- The Sun is where it is: the departure time set in the planner (or the toolbar's **Date (UTC)**, 1976–2050) plus the walking and stop time to the playback position, by Mars24. Light and shadows follow it; after sunset the sky, haze and image-based light fade to night. The inspector shows the sol, local true solar time and Sun, a departure slider, the UTC time and **Earth**: its elevation, azimuth and distance (AU and light-minutes).
- **Earth** is drawn in the sky as an enlarged, labelled dot (from Mars it is a bright point, best seen at dusk or dawn) where JPL's approximate planetary elements and the IAU Mars pole and rotation put it (`marsSky.ts`; the same chain's Sun agrees with Mars24 within 0.3° in elevation). **Look at → Sun / Earth** stands behind the rover or astronaut and faces it.
- Without a departure time (none is ever missing in the planner) the light falls back to a fixed afternoon, labelled.

**Weather** (illustrative, `marsAtmosphere.ts`)
- **Clear**: a dusty daytime sky (pale butterscotch horizon, ruddier zenith, a wide pale aureole around a small sun), haze that thins with height, fine dust drifting in a light breeze.
- **Dusty**: thicker haze and **dust devils**, whirling funnel-shaped columns that wander downwind.
- **Dust storm**: visibility down to a few hundred metres, a dim uniform ochre sky with the sun's disc hidden, dimmer and more diffuse light, streaks of wind-blown dust and drifting curtains of dust across the view.
- Changes ease over a few seconds. Everything moves in the shaders (no per-frame CPU work). Not a forecast or a record for any place.

**Playback and camera**
- Play/pause, Real (152 m/h, Perseverance's top speed) and time-lapse speeds, scrubber, previous/next stop, jump to any point. Playback pauses at science stops with their evidence and a close-up. You can switch routes mid-way.
- **Mission inspector**: a compact panel with the essentials, collapsible details (profile, *At this position*, rover photographs nearby, data and provenance), minimise and close. The **I** key toggles it.
- **Camera**: orbit, **follow**, top-down overview, focus rover (framed to the rover's size) or any stop. Follow tracks the rover across the ground at once but eases up-and-down motion over about half a second, and lifts gently over ridges in the way, so climbing over rocks does not shake the view; the rover itself is never lagged off the ground. Shadows follow the view, snapped to shadow-map texels so they don't shimmer.
- **Data and provenance**: every part of the scene is badged **observed**, **measured**, **computed**, **interpolated**, **illustrative** or **missing**, with source links. Readouts (elevation, slope, grade, data quality, unit or surface class, walking and driving time) come from the planner and the data, never from the playback speed or exaggeration.

**Smooth on every device**
- **Automatic quality**: if the view runs below ~38 fps, it steps down by itself (resolution 1.25 → 1.0 → 0.8 and detail High → Normal → Low) and says so.
- Measured in headless Chrome on an Apple GPU with the frame cap off: High ≈ 165–200 fps, Normal ≈ 213, Low ≈ 221, Off ≈ 450–510; weather costs about 10–25 % (dust storm ≈ 12 %, dusty with dust devils ≈ 22 %).
- If WebGL, the imagery or a model is unavailable, the rest of the planner is unaffected; models have Retry, and missing orbital imagery is announced.

### SOZO Chat

`MissionScientist.tsx` (Mars), `src/components/moon/ChatDrawer.tsx` (Moon), engines in `src/components/sozo/Engines.tsx`

A read-only chat over the current plan, opened with **SOZO Chat** in the header, by clicking SOZO in the corner of the map, or with **SOZO Chat** at the top of the 3D playback (Mars and Moon; the chat opens on the right of the 3D view and keeps the same conversation). **Settings → Answer engine** offers:

| SOZO (recommended) ||
|---|---|
| **Anthropic Claude** | **OpenAI** |
| **Local model** | **Built-in** |

The chat starts on the built-in answers and switches to SOZO as soon as SOZO is connected, unless you picked an engine yourself.

- **SOZO**: paste your SOZO API key in Settings → SOZO (create it at [sozo.momar.si](https://sozo.momar.si/login) → API & developers → API keys). Once connected, SOZO appears in the chat in 3D and answers; see [SOZO, MOMAR's AI scientist](#sozo-momars-ai-scientist).
- **Built-in Mission Scientist** (offline, no key; `src/lib/scientist.ts`, `src/lib/moon/scientist.ts`): a rule-based engine maps a question to one of 14 intents and answers from the plan using the comparison table's own strings, each line labelled (computed, planner fact, interpretation, assumption, caveat, not known; on the Moon measured, derived, assumption, missing). The intents are best route, why lower-risk, time budget, steepest leg, science evidence, rover images, shortest, extra cost, steep ground, risk score, uncertainties, what MOMAR doesn't know, and risk vs time. It never says "safe", only "lower risk under MOMAR's planning assumptions". Missing data is reported as unknown, never zero. When another engine fails, its answer is offered as an explicit choice.
- **Anthropic Claude** (`src/lib/scientistAI.ts`): the official `@anthropic-ai/sdk` from the browser. The default model is `claude-opus-5-5`, with Sonnet 5.5 and Haiku 5.5 available.
- **OpenAI** (`src/lib/openaiCompat.ts`): the Chat Completions API from the browser (api.openai.com answers CORS for browser origins). *Connect* lists the chat models your key can use and selects the newest full GPT model; any listed model can be chosen.
- **Local model** (same module): any OpenAI-compatible server on your computer. *Find local models* looks at Ollama (`localhost:11434`), LM Studio (`:1234`), llama.cpp's `llama-server` (`:8080`), Jan (`:1337`) and vLLM / LocalAI (`:8000`), or at an address you type, and lists their models. The server must allow requests from this site: Ollama with `OLLAMA_ORIGINS=https://www.momar.si`, LM Studio with *Enable CORS*; the browser may also ask to let the site reach apps on this device. Nothing leaves your computer. A reasoning model's `<think>` preamble is hidden.
- Claude, OpenAI and the local model get the same prompt: MOMAR's computed plan data marked as authoritative (`plannerFacts` on Mars, `lunarFacts` on the Moon), with the built-in answer for the question as reference. Every number in a reply is checked against that data and numbers that are not in it are flagged. Their keys are kept in this tab's memory only, never stored, logged or sent to MOMAR's server. A key pasted into the chat box is never sent. Failures are shown as failures, with *Try again* when it can help; nothing is substituted.

---

## SOZO, MOMAR's AI scientist

SOZO (Scientific Observation Zone Operator) is a separate project at [sozo.momar.si](https://sozo.momar.si) (a plain-language guide is in [`docs/sozo/README.md`](docs/sozo/README.md); the prompt for SOZO's next update, from what MOMAR's users saw, in [`docs/sozo/SOZO-UPDATE-PROMPT.md`](docs/sozo/SOZO-UPDATE-PROMPT.md); SOZO's own README in [`docs/sozo/SOZO-README-v0.4.md`](docs/sozo/SOZO-README-v0.4.md), its companion API guide in [`docs/sozo/COMPANION-INTEGRATION.md`](docs/sozo/COMPANION-INTEGRATION.md)). MOMAR stays the route planner and the source of terrain data; SOZO interprets MOMAR's results, compares the routes, looks up literature and answers in plain language. The `/sozo/` page explains it for visitors (navbar: Vision · **SOZO** · How it works).

**What SOZO is sent** (`src/lib/sozo/context.ts`, unit-tested): one function per planner turns the plan on screen into SOZO's companion context:
- the chosen route as a GeoJSON LineString in planetocentric lon/lat degrees (lunar polar-stereographic metres are converted first), at most 400 points with both ends kept;
- only the metrics SOZO has a key for, when the definitions match: `distanceKm` (MOMAR's 3D surface length), `durationHours` (walking time on Mars, mission time on the Moon), `maxSlopeDeg`, `meanSlopeDeg` (Mars), `terrainResolutionM` (20 or 240), and on the Moon `sunlitFraction` and `earthVisibilityFraction` as the share of the route in sunlight or with Earth in line of sight at the planned traversal time;
- the other route as an alternative, so SOZO computes its comparison table from MOMAR's numbers;
- up to 40 planner facts (climb, risk score, steep-ground share, data basis, return to start, vehicle, simulation time, permanent shadow, cold traps, battery, science along the route, "no obstacle catalogue");
- a notes line saying the figures are MOMAR's planner outputs, not measurements or a safety certification.

Non-finite values are dropped; out-of-range values are left out with a fact saying so, never clamped. Screenshots are never sent. The plan is attached only while "The plan on screen" is ticked in SOZO settings, and the settings show the exact data sent with the last question.

**Connecting SOZO** needs nothing on MOMAR's side: no environment variable, no Vercel setting. A visitor signs in at [sozo.momar.si](https://sozo.momar.si/login), creates a key under **API & developers → API keys**, and pastes it in **SOZO Chat → Settings → SOZO**. The `/sozo/` page shows the same three steps.

**Keys never reach the page's scripts** (`src/lib/sozo/server.ts`, `handlers.ts`, routes in `src/app/api/sozo/`): SOZO refuses its API keys from browsers, so the page talks only to MOMAR's proxy:

| Endpoint | Does |
|---|---|
| `GET /api/sozo/status/` | Whether this browser can use SOZO and whose key (MOMAR's, or one saved here, shown as `…abcd`), and SOZO's public status. Never the key. |
| `POST /api/sozo/chat/` | A question (≤ 3,000 characters), the last 10 turns and the plan context → SOZO's `reply`, `citations`, `analysis` (comparison), `limitations`, `contextUsed`. |
| `POST /api/sozo/speech/` | SOZO reads a reply aloud (MP3), for **▶ Listen** and the optional "read answers aloud". |
| `POST` / `DELETE /api/sozo/key/` | Save or forget this browser's own SOZO key (from SOZO settings). |

- **A key pasted in SOZO settings** is first checked with SOZO (a ten-minute session for this origin is opened and revoked at once), then returned to the browser in an `HttpOnly; Secure; SameSite=Strict` cookie scoped to `/api/sozo/`: page scripts cannot read it, the browser sends it only to MOMAR's proxy over HTTPS, and MOMAR's server stores nothing. It is used for that browser's questions until Remove (30 days at most). If the host sets `MOMAR_SESSION_SECRET` (optional), the cookie is also sealed with AES-256-GCM.
- **Optionally**, the site owner can set `SOZO_API_KEY` on the host so visitors without a key can ask too; a visitor's own key always takes precedence.
- Requests to SOZO are server to server: `Authorization: Bearer sozo_…`, JSON, no `Origin`, no visitor cookies, redirects refused, 60 s timeout, none retried on 4xx. A question is retried once, only on 429 or 503: a 502 from SOZO means its AI provider already failed after up to 45 s, and another attempt would only double the wait.
- SOZO's own error codes are passed through (`code`) and explained (`providerMessage` in `server.ts`): for example `provider_unreachable` ("The speech provider could not be reached", which SOZO also returns when its provider, Sarvam, times out) becomes "SOZO's AI service (Sarvam) did not answer: it could not be reached or took longer than 45 s… Try again, turn off research papers for a faster answer, or ask another engine", with *Try again* and the built-in answer offered. The fixes needed on SOZO's side are written up in [`docs/sozo/SOZO-UPDATE-PROMPT.md`](docs/sozo/SOZO-UPDATE-PROMPT.md).
- The proxy answers only requests whose `Origin` is MOMAR's own host, checks sizes and fields before forwarding (a message that looks like an API key is refused), limits each visitor to 10 questions a minute and 60 an hour (in each server instance's memory, so best effort on serverless platforms) in front of SOZO's shared quota (30 a minute, 240 an hour), and returns only the documented reply fields: citations without an http(s) URL are dropped and nothing is rendered as HTML. Message bodies and keys are never logged.
- Tests (`tests/sozoServer.test.ts`) run the handlers against a mocked SOZO and check that no client module can import the proxy, that `SOZO_API_KEY` is read only on the server, and that keys never appear in responses.

**In the planners** (`src/components/sozo/`): quick questions attach the plan (*Explain this route*, *Compare the two routes*, *What are the risks on this route?*, *What data is missing before we rely on this plan?*, *Find research for this site*). An answer shows SOZO's text with its `[n]` markers linked to the retrieved records (unsupported numbers are marked), the comparison table (a value is highlighted only when it is unambiguously better and visibly different), the sources, every limitation, SOZO's great-circle length beside MOMAR's terrain distance, and the label *AI-generated decision support. Verify against MOMAR data and mission rules before operational use.* Failures are shown as failures with SOZO's rejected fields and *Try again* when it can help. SOZO never changes the plan.

**SOZO in the chat**: before a key is connected, the chat shows what SOZO is and the three steps to connect it. Once connected, SOZO appears at the top of the chat in 3D with a short introduction and *Explain this route* / *Compare the two routes*; during the conversation it stays as a slim bar that shows when it is thinking or speaking. **Show SOZO in 3D** (SOZO settings, on by default, remembered in this browser) turns the 3D model off in the chat and over the map, leaving SOZO's monogram.

**Short answers**: MOMAR supplies the terrain and the figures; SOZO analyses them and answers in two to four sentences with the key figures, unless the question asks for detail or an explanation (*explain*, *details*, *elaborate*, *in depth*, *step by step*, *tell me more*…), when it answers in full (`sozoMessage` in `src/lib/sozo/client.ts`; the user's own words are kept and shown as typed).

**Voice**: turn on *Read answers aloud* in SOZO settings, or press **▶ Listen** on an answer, to hear SOZO's own voice (`src/lib/sozo/voice.ts`).

**From the SOZO page**: the *Your AI space scientist* bar (destination, focus, depth, question → **Launch**) opens the Moon or Mars planner with SOZO Chat open and the question filled in (`?ask=…&research=0|1`, removed from the address once read). Nothing is sent until you press Ask, so a link can never spend anyone's SOZO quota.

**The 3D companion** (`SozoAvatar.tsx`, ported from SOZO's own viewer): SOZO's GLB model, loaded from SOZO's public assets (CORS-enabled), in ceramic white and graphite with blue ring eyes that blink and follow the pointer, research lines on its face while it thinks, a voice wave while it speaks and the holographic scanner on its top sensor, which looks straight ahead and sweeps top to bottom, then side to side, only while SOZO works on a question (off otherwise); SOZO's reference image is shown while it loads or if WebGL is unavailable. It renders at most about 30 fps, only while visible, honours reduced motion and frees its GPU resources when closed. On the planners SOZO stands on the map, bottom right, once it is connected (`SozoDock.tsx`; before that, a small SOZO button that opens the chat on SOZO's settings to paste a key), with a holographic ring at its feet. **Click SOZO** and a question box opens at the bottom centre of the map (`SozoMapAsk.tsx`): ask there and SOZO answers in a card above the box, with its comparison table, sources and limitations; *Open in SOZO Chat* shows the same conversation in the chat (the map and the chat share it). **SOZO surveys only when asked to look**: a question that asks it to observe, scan, survey, look around or check the map, rocks, craters, obstacles or hazards (`wantsSurvey` in `src/lib/sozo/client.ts`) sends SOZO walking over the map: it moves left and right, stops, and at each stop projects a holographic scan field in a different direction (beside it, the other side, the ground in front), swept top to bottom, then side to side; then it walks back to its place and the answer appears. Other questions it answers where it stands, thinking. The tour is decoration: SOZO answers from MOMAR's data, not from the map image. **In the 3D playback** (Mars and Moon) SOZO stands in the bottom-right corner over the 3D view with the same question box, above the playback controls. Drag SOZO anywhere, or move it with the arrow keys (Home puts it back); minimise it to SOZO's monogram; its place is remembered in this browser. While the chat is open SOZO is shown inside it, so the one on the map steps aside; on phones it starts above the bottom sheet and rises above the question box.

---

## MOMAR Lunar (the Moon)

A separate planetary system in the same product: its own pages, code (`src/lib/moon/`, `src/components/moon/`, `src/workers/moon.worker.ts`), data (`public/data/moon/`) and pipeline (`scripts/build_moon.py`, `scripts/moon/`). The Mars code, data and assumptions are untouched; the only shared piece reused is the generic A\* search (`src/lib/pathfind.ts`, imported unchanged).

### Frames and conventions

- Body-fixed frame **MOON_ME** (mean Earth / polar axis), the frame of the LOLA polar products (JPL DE421) and of JPL Horizons' lunar orientation (DE441, "MOON_ME high precision"). Latitude planetocentric, longitude east-positive. Reference sphere **R = 1737.4 km**: every height is metres above it.
- **South polar stereographic** (`+proj=stere +lat_0=-90 +lat_ts=-90 +lon_0=0 +R=1737400`) for the workspace and its grids, exactly the LOLA and USGS products' own grid. The USGS ice favorability index uses `lat_ts = −70`: it is reprojected, never treated as the same map (a test checks they differ by > 4 km at 85°S). Forward and inverse formulas match PROJ to 1 mm (`tests/moon/frames.test.ts`).
- Distances and bearings are great-circle on the sphere; projected metres are divided by the map scale factor (≤ 1.0028 in the workspace). Azimuths are clockwise from local north (towards the north pole along the site's meridian).

### Sun and Earth: where they really are

- `scripts/moon/ephem.py` asks **JPL Horizons** (DE441, MOON_ME) for the apparent azimuth, elevation and range of the Sun and of Earth from a reference site on the Moon, every 6 hours from 1966 to 2045, and stores each body's position from the Moon's centre (`public/data/moon/ephem/<year>.bin`, ≈ 33 kB per year, loaded per year as needed). The browser interpolates (4-point Lagrange) and does the topocentric geometry itself: the observer's offset from the Moon's centre moves Earth by up to 0.26°.
- **Verified against Horizons' own topocentric directions** at 9 sites (south pole region, far side near the pole, Malapert, the sub-Earth point, Apollo 11, far-side centre, east limb, Chang'e 4, northern near side) × Sun and Earth × 6 epochs (1969-07-20 to 2035): worst error **0.00002°** in elevation and azimuth (test tolerance 0.0002°). Times outside 1966–2045 are refused, never guessed.
- (A first version used Horizons' sub-Earth point *as seen from Earth*; the test caught a 0.0046° error from Earth's aberration, and the table was rebuilt from the Moon's side.)

### Terrain horizons, shadows, illumination, Earth line of sight

- **Horizon** (`horizon.ts`): rays along great circles every 0.5° of azimuth, out to 250 km, in exact 3-D (no flat-ground approximation), reading the finest DEM that covers each point: the 240 m workspace DEM, then the 960 m far-field DEM to ≈ 70°S. Its ±1σ band comes from LOLA's elevation error at the occluding point. Rays that leave all DEMs are flagged (the bare sphere is assumed beyond).
- **Visibility**: the visible fraction of the Sun's (0.53°) and Earth's (≈ 1.9°) disc above the horizon at their azimuths. "Sunlit" = any part of the Sun above the terrain; Earth "visible" = geometric line of sight to any part of its disc, explicitly **not a communications window**.
- **Timelines** (`illumination.ts`): samples over 1 day to 1 year, every transition refined by bisection to one minute: sunlit share, fully sunlit share, mean visible disc, longest darkness (flagged when it runs past the interval), Earth line-of-sight share, next sunrise/sunset and next Earth-rise/set. A long sunlit period is never called "permanently illuminated".
- **Shadow maps** for any time (`analysis.ts`): from every other workspace cell, a straight 3-D ray towards the Sun across both DEMs; the terrain angle above the ray in its vertical plane gives the visible fraction of the solar disc. Computed in the worker in ≈ 7 s; the same function lights the 3D view.
- Tested on synthetic terrain with exact answers (bare-sphere dip for a 2 m eye, a mountain at a known distance and bearing, a ridge's shadow and its length, sunset/sunrise refinement).

### The south-pole planner (`/moon/south-pole/`)

Laid out like the Mars planner: a header with a region menu (named features), the *Archived scientific data · not live* badge, the time, How it works, Vision, **SOZO Chat**, **3D** and **Mission plan**; six tabs (a bottom sheet on phones; on wider screens they fold to a rail of symbols, as on Mars); the map or the 3D view beside them, and SOZO on the map with its question box once SOZO is connected.

- **Map** on the polar stereographic grid (Leaflet, flat CRS in metres): **LROC NAC 1 m controlled mosaic** or **ShadowCam 1 m mosaic** streamed from USGS cloud-optimised GeoTIFFs at the zoom level's pixel size (a small COG reader, `src/lib/geotiff/cog.ts`: BigTIFF, deflate, predictor 2, overviews, lazy tile tables; checked against GDAL-written files and live against rasterio), or LOLA hillshade. A hover readout (from the worker) gives elevation, slope, PSR share, long-term Sun and Earth visibility, Diviner maximum temperature and ice depth, and the terrain data's quality. Routes: lower-risk orange, shortest blue dashed, return violet dotted; a stop that had to move to passable ground is joined to where it was asked for.
- **Plan** (as on Mars): *1 · Where should I go?* A and B by Pick, latitude/longitude or a named-feature search, up to 8 stops (reorder, remove), and **routes suggested from the data** around the selected point (best-lit ground to the nearest Diviner cold trap; between the two best-lit high points; from ground with Sun and Earth in view to the shallowest modelled buried-ice stability), each point with the values that chose it. *2 · How can I get there safely?* Routes are planned as soon as A and B are set; both are compared side by side with the difference (distance, drive and mission time, climb, steepest cell, permanent shadow, cold traps, rough ground, poorly covered terrain, light and Earth on arrival, long-term sunlight, farthest point from A), then the chosen route's legs, slope classes, what makes up its risk, first shadow, battery (your figures), return to A, the Apollo LRV range record, and the profile with sunlight and Earth line of sight. *3 · What will I find along the way?* The nearest cold trap, the shallowest modelled ice outside cold traps, permanently shadowed ground, the highest ice favorability index and the best long-term sunlit and Earth-visible ground within 1.5 km, each with its evidence, source, data quality, kind (measured, model, index) and limits, and *Add as stop*. A bottom bar: Mission plan, GeoJSON, 3D view.
- **Conditions**: elevation, slope and aspect, **5 m slopes** read live from the LOLA 5 m DEM inside 87°S, roughness, PSR share, ice favorability (an index, not ice), long-term sunlight and Earth visibility, Diviner maximum and mean temperatures and ice stability, data quality, geologic unit; **Sunlight** and **Earth Visibility** now (azimuth, elevation, terrain clearance, status, next transition, uncertainty); a **sky view**; adjustable eye height; and the Sun and Earth **over time** (timeline, summary, sky paths, light periods).
- **Sites**: candidate sites ranked by stated rules and editable weights (gentle slopes, smooth ground, sunlight, Earth line of sight, data quality, nearness to a PSR); every value, score and weight shown; unevaluated criteria left out, not scored zero. Decision support, never "certified" or "safe".
- **Images**: LROC NAC images whose footprints cover the selected point, queried live from the PDS Orbital Data Explorer (finest, newest or best-lit first), with the footprint on the map, the browse strip and a link to the LROC viewer.
- **Layers**: base imagery; layers in groups (terrain; light and Earth; resources and temperature; orbital evidence, reference only; data quality; context) with search, per-layer opacity, restore defaults, the reason when a layer is not in a build, and every dataset's source, resolution and limits.
- **Assumptions**: mobility presets with every value cited or marked as an assumption (`src/lib/moon/mobility.ts`): **Apollo LRV-class** (2.30 m/s, the Apollo 15–17 mean while driving, NSSDCA; 20° on 240 m slopes, designed for 20–23°, Jacob et al. 2026; driving half the time, the LRV sortie profile; 7.6 km range record), **VIPER-class** (10 cm/s, 0.8 cm/s made good, 15°; NASA), **astronaut on foot** (0.8 m/s, just above the suited walk-to-lope transition, Carr & McGee 2009) and the generic default; every parameter editable; energy (your figures) and return checks; eye height.
- **Timeline**: one traverse timeline (`src/lib/moon/traverse.ts`) gives the planner its arrival times (for light and Earth along the route) and plays the 3D view: an LRV-class traverse drives at speed and spends the rest of the time at its stops (as Apollo sorties did); a robotic one moves in short drive-and-stop cycles.
- **3D mission playback** (full screen, as on Mars: `Moon3D.tsx`, `moonScene.ts`, `MoonPlaybackPanel.tsx`):
  - *Header and toolbar*: the data line, model credits and Close; what moves on the route (**Rover**, **Astronaut**, **Both**: the LRV drives and the crew walks out at each stop, or an astronaut walks with a robotic rover escorting 8 m behind), the vehicle (Apollo LRV or a generic robotic rover), the camera (Orbit, Follow, Overview), Focus, Reset view, Look at Earth or the Sun (from the vehicle at 1.7 m), vertical scale (1–3×, flagged), illustrative detail (High, Normal, Low, Off), ground (computed light or the LROC NAC image), the sky (Sun and Earth shown or hidden) and the **Date** (UTC, ±1 h/±1 d steps, limited to the Horizons table's 1966–2045), which moves the Sun, Earth and the computed light. The camera never goes under the ground: it is held at least 1 m above the drawn surface and never orbits below the target's horizon. Space plays and pauses; Escape closes.
  - *Ground*: far terrain from the 240 m DEM, each vertex lit by ray-marched sunlight; a 2.6 km patch of the **LOLA 5 m DEM** around the vehicle (inside 87°S; elsewhere the 240 m DEM interpolated, labelled) with **GPU shadows** from the same Sun direction, multiplied by the far-field shadow; within 80 m, **illustrative small craters** (N(≥D) ∝ D⁻², the equilibrium slope of small lunar craters, Gault 1970), gentle undulation and **rocks** sized by the Golombek & Rapp model at an assumed lunar rock abundance (Diviner finds most of the Moon under 1 %, Bandfield et al. 2011), kept out of the vehicle's track and the crew's walks (`src/lib/moon/regolith.ts`); wheel tracks pressed behind the wheels.
  - *Motion*: the **Apollo LRV** (Deutsches Museum model, CC BY-SA 4.0) rolls by distance ÷ radius, steers both axles within the LRV's 3.1 m turning radius and keeps every wheel on the drawn ground; **dust** leaves the tyres (and the astronaut's boots on landing) and flies exact ballistic arcs in lunar gravity with no air, falling straight back with no haze (`src/lib/moon/dust.ts`). The astronaut walks below the suited walk-to-lope transition (Froude 0.36) and lopes above it, each flight a lunar-gravity parabola (`src/lib/moon/lunarGait.ts`; the model's own clip floats far slower than lunar gravity, measured in the tests). The traverse plays on the planner's timeline (a walker on its own at EVA pace), at real time to 3600×.
  - *Mission inspector*: lower-risk or shortest route; ⏮ ◀ Play ▶ ⏭; Real, 10×, 60×, 600×, 3600×; jump to a stop; a scrubber with distance and %; the leg, the time and the simulated UTC; at a stop, "Leave now". Details: the elevation profile with the position marked; **at this position** (coordinates, 5 m DEM elevation, drawn and 240 m slopes, vehicle tilt, permanent shadow, long-term Sun and Earth, Diviner temperature and ice stability, sunlight at the vehicle, Sun and Earth now, LOLA data quality, the astronaut's gait figures, time); **LROC images here** (ODE, on request); data and provenance (measured, interpolated, illustrative); about this view. It minimises to a bar or a "Mission inspector" pill; playback never depends on it.
- **SOZO Chat**: SOZO with this workspace's routes as context ([above](#sozo-momars-ai-scientist)), or Claude, OpenAI or a local model with your own key or server ([SOZO Chat](#sozo-chat)), given MOMAR's computed lunar values and the built-in answer, with every number in the reply checked against them; or the built-in Mission Scientist itself (deterministic, offline, every statement labelled measured / derived / assumption / missing, never "safe" or "confirmed ice").
- **Mission plan**: a dark report (Save as PDF keeps the colours; a light print option) of the chosen route, both routes' figures, science along the way, the site, the timeline, the comparison, the mobility preset with its sources, the data sources and the limits; GeoJSON of the routes and stops.

### The whole Moon (`/moon/`) and the catalogue (`/moon/missions/`)

- Quadtree globe of the **LROC WAC** mosaic (4096 × 2048 build-time base, then Moon Trek tiles to ≈ 83 m/px; refinement stops at 75° latitude, where the workspace takes over). IAU names (2,045), located hardware by category, **USGS Unified Geologic Map** (1:5M, colours by period), **Kaguya MI composition** (FeO, OMAT, olivine, plagioclase, clinopyroxene, orthopyroxene; 50°N–50°S), sunlight for any time with the sub-solar and sub-Earth points.
- Click: **LOLA elevation** read live from the 118 m global DEM (one range request; the Apollo 11 LM reads −1,927 m, LROC lists −1,926 m), the containing feature, geologic unit, composition, Sun and Earth geometric elevation, hardware within 30 km, and **Open in the south-pole workspace** with the same point and time.
- **Catalogue**: the LROC anthropogenic objects (71 located objects: Apollo LMs, ALSEPs, retroreflectors and the three LRVs, Lunokhod 1 and 2 by laser ranging, Luna, Surveyor, Chang'e 3/4/5/6 and Yutu, Chandrayaan-3 Vikram, SLIM, IM-1, IM-2, Blue Ghost; impact sites of Beresheet, Chandrayaan-2 Vikram, HAKUTO-R 1 and 2, Luna 25; intended impacts and spent stages), plus Pragyan, Yutu-2, LEV-1 and LEV-2 (positions not published: shown with their landers) and planned VIPER, Chang'e 7 and NASA's Lunar Terrain Vehicles (never placed on the Moon; target regions only). No historic traverse is drawn.

### Lunar data

| Product | Source | As used | Status |
|---|---|---|---|
| LOLA south pole DEM, error, effective resolution, counts | PGDA, Barker et al. 2023 (doi:10.3847/PSJ/acf3e1) | 240 m, ±182.4 km (exact 3 × 3 means of 80 m bins) | built in |
| Permanently shadowed regions | PGDA LPSR_80S_20MPP_ADJ | share of each 240 m cell (12 × 12 bins of 20 m) | built in |
| Roughness (RMS, 100 m baseline) | PGDA LDRM_80S_1000MPP | 1 km | built in |
| Far-field DEM | PGDA LDEM_60S_240MPP | 960 m, ±576 km | built in |
| LOLA 5 m DEM, 87–90°S | Barker et al. 2021; USGS COG | 5 m pixels (effective resolution varies) | streamed |
| LROC NAC south pole mosaic, ShadowCam mosaic | NASA/ASU, KARI; USGS LMAP COGs | 1 m mosaics, read at the view's pixel size | streamed |
| Ice Favorability Index | USGS Astrogeology | 591 m, reprojected | built in |
| LOLA global DEM | USGS mosaic | 118 m, point reads | streamed |
| LROC WAC global mosaic | NASA/ASU via Moon Trek | 2.7 km base, tiles to 83 m | built in + streamed |
| Kaguya MI mineral maps | JAXA, Lemelin et al.; USGS | 4 px/deg, 50°N–50°S | built in |
| Unified Geologic Map of the Moon | USGS, Fortezzo et al. 2020 (CC0) | simplified to 2.4 km / 250 m | built in |
| IAU nomenclature | IAU / USGS Gazetteer | 2,045 features | built in |
| Human-made objects | LROC (Wagner et al. 2017; updated 2026-08-14) | 71 located objects | built in |
| Sun and Earth | JPL Horizons, DE441, MOON_ME | 1966–2045, 6-hourly | built in |
| Long-term Sun and Earth visibility | LOLA EXTRAS AVGVISIB_65S_240M_201608 (+ _EARTH), Mazarico et al. 2011 | 240 m, same grid edges; orientation checked against the PSR map | built in |
| Diviner Polar Resource Product | PDS4 DLRE_PRP_SOUTH, Paige et al. 2010 | 1.04 M model triangles binned to 480 m: maximum and mean temperature, depth of ice stability | built in |
| LROC NAC image index | PDS Orbital Data Explorer (CDRNAC4) | footprints and metadata for the selected point | streamed |
| LEND neutron map (hydrogen indicator) | PDS Geosciences, LRO-L-LEND-4/5-RDR-V1.0, LEND_RDR_ALDS, Mitrofanov et al. 2010 | 0.5° bins, 2009-09 to 2016-09; exposure-weighted Gaussian average (σ 7.5 km) drawn on 480 m; suppression from the 80–82°S ring. Potential hydrogen, never confirmed water ice | built in, reference only |
| Mini-RF radar CPR | PDS Geosciences, LRO-L-MRFLRO-5-GLOBAL-MOSAIC-V1.0, 128 ppd, 2009-07 to 2010-12, read by range requests | 480 m means. High CPR is ambiguous: rough ground or ice | built in, reference only |
| LAMP far-UV albedo (Lyman-α) | PDS Imaging, LRO-L-LAMP-5-GDR-V1.0, doi:10.17189/1520244, Gladstone et al. 2012 | 240 m source, 480 m means, 2009-09 to 2010-10; orientation checked against the PSR map. Not an ice map | built in, reference only |
| CRaTER radiation reference | University of New Hampshire legacy L30 daily dose rates (PDS PPI holds the raw data), Spence et al. 2010 | yearly summary in `crater.json`, 2009-06 to 2012-12. Silicon dose in lunar orbit, not a dose on a route | built in, archived |
| NAIF SPICE cross-check | NASA NAIF generic kernels (DE440s, DE440 lunar frame, PCK00011, NAIF0012), SpiceyPy | apparent Sun and Earth directions at the Horizons test's sites and epochs, in `tests/fixtures/spice/directions.json` | check at build and test time; the app does not read it |

**Not integrated** (and said so in the app): M³, GRAIL, SLDEM2015, the shape-from-shading 5 m DEMs (Zenodo), historic traverses, and any north-pole workspace.

The Diviner table uses −999 for "deeper than 2.87 m" (as its label says) and also −1.0, which the label does not document: on 6.7 % of the workspace's triangles, all cold traps (annual maximum below 101 K). MOMAR leaves −1.0 out of the depth statistic as unknown; the temperature layer still shows those places as cold traps.

**Rebuilding** (`data/raw/moon/` cache, git-ignored): `.venv/bin/python scripts/build_moon.py` runs `ephem`, `fixtures`, `southpole`, `catalogue`, `globe`, `geology`, `resources` (LOLA long-term visibility, two 19.5 MB range reads; the 605 MB Diviner table), `evidence` (the two LEND tables, 212 MB of Mini-RF range reads, 25 MB of LAMP, the CRaTER table); any subset can be named. PGDA sends no CORS headers, so its products are reduced at build time (≈ 15 min over a slow link); the USGS and Moon Trek products stream in the browser.

### Lunar limitations

- 240 m terrain for routing and statistics (5 m for site slopes and the 3D close range inside 87°S): boulders, small craters, small shadows and small sunlit peaks are not resolved; routes and site statistics are regional.
- Illumination and Earth visibility are geometric: no light scattered from terrain, no earthshine; horizons stop at 250 km and at the DEMs' edges (flagged). Long-term visibility is an 18.6-year average, not the light at a chosen time.
- No resource is confirmed: PSR, Diviner cold traps and ice stability, and the ice favorability index say where ice could survive, not where it is. Diviner's model is ~0.5 km and fitted to its first mapping year.
- Mobility presets are planning values from public records (cited) or MOMAR assumptions, not flight specifications; VIPER was cancelled before launch.
- 3D: the LRV is a museum replica drawn at its own size (wheelbase 1.96 m against the flown 2.3 m); the wheels follow the ground but there is no suspension physics, slip or sinkage; the crew is not drawn while riding (the model has no seated pose); the astronaut's gait is the artist's clip with lunar-gravity flights, not spacesuit biomechanics; the craters, rocks, ground texture, tracks and the number and angles of dust grains are illustrative (the grains' flight is exact physics); Earth's surface pattern is illustrative (its position, size and phase are computed).

---

## How it works

```
scripts/ (Python)                          public/data/<region>/            browser
 build_region.py ── dem, ortho, geology ─▶  manifest.json + .bin grids ──▶  Planner (React)
                    crism, traverse, …      WebP tiles, GeoJSON, JSON       ├─ Leaflet, Mars equirectangular CRS
 build_globe.py ── globe colour, relief ─▶  public/data/mars/globe/         ├─ Web Worker: slope · risk · Theta*/A* · shadows · overlays
                                                                            └─ Three.js: globe, 3D playback
 USGS S3 archive + OnMars tiles ─────────── read live in the browser ──────▶ globe sites (manifest built in the browser)
```

- **Slope**: Horn (1981) 3×3 gradient, east–west cell size corrected per row (cell × cos φ). **Roughness**: RMS residual about the Horn plane; where HiRISE was sampled, a 2 m-baseline P90 slope adds detail. `src/lib/terrain.ts`
- **Risk** (0–1 per cell): weighted (slope/limit)², roughness, HiRISE fine slope, loose ground (mapped eolian bedforms in Jezero, night-IR dust and dark sand at sites) and data quality. Cells above the slope limit or without data are impassable. `src/lib/cost.ts`
- **Routes**: *shortest* minimizes 3-D distance, *lower-risk* minimizes Tobler walking time × (1 + risk aversion × risk). Each leg is searched twice on the same passability mask: **Theta\*** any-angle search (`anyAngle.ts`; vertices in a 16-cell corridor around the A\* path, edges up to 32 cells, each edge costed and checked by the exact supercover segment cost, so line of sight is never assumed and corner contacts count), and the original 8-connected A\* without corner cutting, string-pulled where every touched cell is passable and it costs no more. Both get the same corner smoothing and are costed by the same exact segment cost; the cheaper is kept, the A\* route on a tie, and both searches' cost, length, turning, run time and expansions are reported (`route.search`, also in GeoJSON as `route_search`). On the Jezero scenarios Theta\* lowers the lower-risk routes' cost by 1.1–2.6 % and the shortest routes' by 0.1–0.7 %; typical routes plan in 80–200 ms, a 28 km crater crossing in about 2 s. Statistics are cell-based; distance, climb and time use points every ≤ 10 m. Multi-stop legs are joined without repeating junctions; a third search plans the return. `pathfind.ts`, `route.ts`, `traverse.ts`, `itinerary.ts`
- **Terrain-aware smoothing** (`relaxPath` in `route.ts`): on coarse grids string pulling leaves 45° kinks wherever a straight cut would cost more. A validated corner cut follows (Chaikin, up to 3 passes): at each corner the chord replacing it must touch no impassable or out-of-grid cell (exact supercover: slope limit and blocked terrain hold) and cost at most 0.5 % more under the route's own model. Itinerary points never move. The whole route must cost at most 1 % more and every segment must pass again, otherwise the string-pulled path is kept. Every statistic is computed on the smoothed route; the grid path is kept with its length for comparison. Tests check endpoints and stops, the independent route-safety oracle around walls and gaps, the cost bound and the fallback. At a Gale site the lower-risk route to the lookout came out 148 m shorter than its grid path.
- **Validation against Perseverance** (`traverseValidation.ts`, `tests/traverse-validation.test.ts`): for three presets that are the rover's own stops (sols 13 → 420, 700 → 797, 1204 → 1400), every vertex of the rover's recorded drive lies on ground MOMAR's cost grid lets an astronaut walk; the planned routes are 24–71 % of the rover's driven length (the rover drove for science); on the fan and the rim the lower-risk route keeps closer to the rover's ground (mean 95–118 m) than the shortest (125–280 m). A sanity check of the terrain model, not a validation of an astronaut planner.
- **Mars time and the Sun**: Mars24 (Allison & McEwen 2000) reproduces both NASA GISS worked examples; Perseverance's sol and mission clock match NASA's records for all 626 indexed rover images (every sol; LMST within 5–20 s, one image 122 s). Terrain shadows: an O(N) sweep from the sun side for the map layer and a ray march for route points (agreeing on 99.6–100 % of cells), both with Mars's curvature (d²/2R, 59 m at 20 km).
- **Comparison**: `compare.ts` builds the table and the explanation from the computed results only; tests check every number in the text appears in the table.
- **Science**: CRISM parameters must exceed max(noise-floor assumption, regional 95th percentile) within 100 m of the route; geologic contacts (excluding surficial eolian cover) persist ≥ 40 m; at sites, entries into night-warm ground held over ~2.5 cells. `science.ts`
- **Globe sites**: `site.ts` reads the windows (`globalData.ts`: TIFF/BigTIFF layout, georeferencing, nodata, wrapping, retries), classifies the surface, and `siteRegion.ts` writes a planner manifest with streamed layers, provenance and data-driven presets. The planner worker then derives slope, roughness and risk exactly as for Jezero.
- **Tiles**: `marsTiles.ts` (geographic 512 px tiling on the Mars 2000 sphere: level L tiles span 180/2^L degrees; MDIM to level 7, CTX to level 12) serves the globe, the site map (Leaflet `tileSize 512, zoomOffset −1`), the 3D ground and the report. Tiles are stitched with pixel-snapped edges so no seams show; truncated tiles still decode.

Design notes: route colours were checked with a CVD/contrast validator (blue `#3987e5` dashed = shortest, orange `#e0622c` solid = lower-risk, violet = return). Charts have legends, crosshair tooltips and table views. The site respects `prefers-reduced-motion` and is keyboard accessible.

---

## Data

### Jezero build

| Dataset | Source | Resolution | Status |
|---|---|---|---|
| CTX DTM, Mars 2020 TRN | USGS / MRO CTX · [doi:10.5066/P906QQT8](https://doi.org/10.5066/P906QQT8) | 20 m (1596 × 1512 grid, 77.16–77.70°E, 18.21–18.72°N) | **Real** |
| CTX orthomosaic, Mars 2020 TRN | USGS / MRO CTX · [doi:10.5066/P9GV1ND3](https://doi.org/10.5066/P9GV1ND3) | 6 m (tiled to ~5 m) | **Real** |
| HiRISE DTM mosaic, Mars 2020 TRN | USGS / MRO HiRISE | 1 m source, sampled; 31 % of cells | **Real (sampled)** |
| HiRISE orthomosaic, Mars 2020 TRN | USGS / MRO HiRISE · [doi:10.5066/P9QJDP48](https://doi.org/10.5066/P9QJDP48), via NASA Trek | 25 cm | **Streamed** |
| CRISM MTRDR summary parameters (8 obs, 2007–2011) | PDS Geosciences / MRO CRISM | 18 m (FRT), 36 m (HRL); 67 % of cells | **Real** |
| Geologic map (SIM 3464, Sun & Stack 2020) | USGS ArcGIS feature service | 1:75,000 | **Real** |
| Perseverance traverse & waypoints (sols 13–1980) | NASA/JPL MMGIS feed (updated 2026-09-15) | rover localization | **Real** |
| MEDA air temperature & pressure (24 sols, 1191–1835) | PDS Atmospheres, mars2020_meda | 1 Hz, sampled | **Real (archived, sampled)** |
| Perseverance raw images (626, sols 1204–1400) | NASA/JPL Mars 2020 raw-image feed; credit per image | 320 px thumbnails, full size linked | **Real (curated)** |
| IAU nomenclature | USGS Gazetteer | points | **Real** |
| THEMIS day IR, MOLA colour hillshade | via NASA Trek | 100 m, 463 m | **Streamed** |
| MOLA DEM gap-fill | USGS / MGS MOLA | 463 m | **Fallback, not needed** |
| Ionizing radiation | — (Perseverance has no energetic-particle detector) | — | **Unavailable**: a Curiosity RAD reference (≈ 0.67 mSv/day, Gale) is shown and labelled |
| THEMIS thermal inertia, quantitative mosaic (tile 00N060E) | USGS Astrogeology / ASU (Fergason et al. 2006), ISIS cube read by byte range (9 tiles, 0.6 MB) | 100 m; ±20 % absolute | **Real** (median 341 TIU) |
| TES thermal-inertia map, nightside | PDS Geosciences, MGS-M-TES-5-TIMAP-V1.0 (Putzig & Mellon 2007) | 20 px/° (≈ 3 km) | **Real** (median 300 TIU) |
| SHARAD ground tracks and archive links | PDS Orbital Data Explorer, MRO-M-SHARAD-4-RDR-V1 (Seu et al. 2007) | tracks drawn as lines; radar about 15 m vertical in free space, 3–6 km across-track. No subsurface interpretation | **Real** (reference only) |
| MCS archived climate, Mars Year 36 | PDS Atmospheres, MRO-M-MCS-5-DDR-V6.2, doi:10.17189/6khc-s052 | ±10° latitude, ±20° longitude, 30° of Ls, 5 sols per bin; not a forecast | **Real** (archived, sampled, reference only) |

### Gale build

| Dataset | Source | Resolution | Status |
|---|---|---|---|
| MSL Gale merged DEM v3 (HRSC, CTX, HiRISE, tied to MOLA) | USGS Astrogeology / PDS Annex (Calef & Parker 2016), 3.6 GB, read one 1 m row per 20 m cell by range requests | 1 m source, 20 m grid (889 × 1392, 137.25–137.55°E, 4.95–4.48°S) | **Real (sampled)**: matches Curiosity's 1386 localized elevations to a median 0.0 m (90 % within ±1.5 m) |
| CTX global mosaic (Murray Lab), tiled locally | Esri OnMars tiles (level 12 = the planner's zoom 13 pixel grid) | ≈ 5 m | **Real** |
| CRISM MTRDR summary parameters (15 obs) | PDS Geosciences / MRO CRISM | 18–36 m; 96 % of cells | **Real** |
| Curiosity traverse & waypoints (sols 3–5045, 38.5 km) | NASA/JPL MMGIS feed; each drive takes the sol of its drive-end waypoint | rover localization | **Real** |
| THEMIS (tile 30S120E) and TES thermal inertia | as for Jezero | 100 m / 3 km | **Real** (medians 384 / 283 TIU) |
| IAU nomenclature, Bradbury Landing, Curiosity's latest position | USGS Gazetteer; MSL localization feed | points | **Real** |
| HiRISE stereo DTMs (DTEEC), read by range requests | University of Arizona, PDS MRO-M-HIRISE-5-DTM-V1.0 | 1 m source, one 2 m-baseline transect per 20 m cell; each DTM checked against the merged DEM | **Real (sampled, reference only)**: route risk does not read it |
| SHARAD ground tracks, MCS archived climate | as for Jezero | as for Jezero | **Real** (reference only) |
| Geologic map | — no detailed USGS map service for the area | — | **Unavailable** (science stops from CRISM only) |
| HiRISE fine slope in route risk | — the merged DEM has no source mask, and using the separate DTMs would change routes before that is validated | — | **Unavailable** for routes (reference layer above) |
| Weather (REMS), rover photographs | — not integrated | — | **Unavailable** |

### Whole Mars and globe sites

| Dataset | Source | Resolution | Use |
|---|---|---|---|
| Viking MDIM 2.1 colour mosaic | USGS (NASA Viking) | 925 m → 8192 × 4096 base; 231 m tiles | Globe colour, site hue, albedo for dark sand |
| CTX global mosaic V01 | The Murray Lab/Caltech (NASA/JPL/MSSS), via Esri OnMars tiles | ≈ 5 m | Globe and site imagery, 3D ground, report map |
| MGS MOLA DEM | USGS / MOLA Science Team | 463 m | Globe relief, live elevation, site fallback |
| MOLA – HRSC blended DEM v2 | USGS Astrogeology (originators DLR, NASA GSFC) | 200 m | Site terrain |
| THEMIS night-time IR (60°S–60°N) | Mars Odyssey, ASU / USGS | 100 m | Site surface classes |
| THEMIS day IR | ASU / USGS | 100 m | Site map layer; imagery fallback |
| IAU nomenclature (219 features) | USGS Gazetteer | points | Globe labels, site points of interest |
| MOLA elevation texture (2048 × 1024) | USGS / MOLA 463 m DEM, block means | ≈ 10 km | Globe relief in profile (flat, true, 3×, 10×) |
| TES thermal inertia, 162 tiles of 20° | PDS Geosciences (Putzig & Mellon 2007), re-encoded by `scripts/build_tes.py` (8.4 % log steps, infill flag) | ≈ 3 km | Site thermal inertia layer and readout |

Globe base maps are built once by `scripts/build_globe.py` (see below); everything else is read live in the browser.

---

## Real, sampled, assumed, illustrative

- **Real**: imagery, terrain, minerals, geology, traverse, photographs, weather records, surface classes and names all come from the archives above.
- **Sampled**: the HiRISE DTMs (one transect per 20 m cell), MEDA (byte-range windows) and MCS (a few sols per season, surface lines only), labelled as samples.
- **Reference only**: LEND, Mini-RF, LAMP, CRaTER, SHARAD, MCS and the HiRISE slopes at Gale are shown beside the plan. No route or risk score reads them; tests check that. LEND is *potential hydrogen*, high Mini-RF CPR is ambiguous, LAMP is an albedo, and none is shown as confirmed ice.
- **Relative**: globe-site surface classes are relative within each site (a proxy, not calibrated thermal inertia); measured-derived thermal inertia from THEMIS and TES is shown separately, with its uncertainty.
- **Assumed**: slope limit 25°, walking speed 2.5 km/h, Tobler's function, risk weights, CRISM noise floors, EVA budget 4 h, 20 min per science stop, rock abundance 5 %, outcrop density. All are visible, editable or documented, and labelled *planning assumptions, not NASA suit or EVA limits*.
- **Illustrative**: rocks, outcrops, fine relief, gravel, slabs, strata, wheel tracks, dust, weather, sky and lighting, the astronaut's gait and the rover's motion. All are labelled in the 3D view and never used by the planner.
- **Never**: fabricated telemetry, live data, invented coordinates, or fake form success.

---

## Regenerating the data

Requirements: Python ≥ 3.11 (GDAL comes with the rasterio wheel).

```bash
python3 -m venv .venv
.venv/bin/pip install -r scripts/requirements.txt
.venv/bin/python scripts/build_region.py jezero                  # all steps
.venv/bin/python scripts/build_region.py gale                    # the second region (DEM by range reads, ~2 min)
.venv/bin/python scripts/build_region.py jezero --steps thermal,manifest
.venv/bin/python scripts/build_globe.py                          # globe base maps (~1 GB of range reads, cached)
.venv/bin/python scripts/build_globe.py --elevation-only         # just the MOLA relief texture, from the cache
.venv/bin/python scripts/build_tes.py                            # TES thermal-inertia tiles for globe sites (78 MB download, cached)
.venv/bin/python scripts/build_region.py jezero --steps sharad,mcs,manifest    # radar tracks, archived climate (shared cache, range reads)
.venv/bin/python scripts/build_region.py gale --steps sharad,mcs,hirise_ref,manifest
.venv/bin/python scripts/build_moon.py evidence                  # LEND, Mini-RF, LAMP, CRaTER
.venv/bin/python scripts/build_spice.py                          # SPICE cross-check table for tests/spice.test.ts (kernels cached, about 50 MB)
npm run numbers                                                  # rewrite "MOMAR in numbers" from the manifests
```

Downloads are cached in `data/raw/` (git-ignored); outputs go to `public/data/`. Each step writes `meta/<step>.json` with provenance and a **real / fallback / unavailable** status; a failed step is recorded as unavailable and the app hides or labels that layer. A region config can list steps it has no data for under `unavailable`, each with the reason the app shows. The original 4K moon video is kept outside git in `assets-src/video/` (over GitHub's 100 MB limit); `public/videos/moon.mp4` is a 720p web copy made with macOS `avconvert`.

| Step | Source | What it produces | Notes |
|---|---|---|---|
| `dem` | USGS Mars 2020 TRN **CTX DTM** (20 m) | `dem.bin` (int16, 0.1 m), `dem_source.bin` | Gaps filled from **MOLA 463 m** via range reads |
| `ortho` | USGS Mars 2020 TRN **CTX orthomosaic** (6 m) | WebP tile pyramid z8–z13, overview, hero | NASA Trek equirectangular tiling |
| `geology` | **USGS SIM 3464** feature service | GeoJSON, unit grid, surface features, landing ellipse | Inverse spherical transverse Mercator done analytically |
| `crism` | **CRISM MTRDR** via PDS ODE | parameter grids, composites, footprints | Only 8 bands fetched with Range requests |
| `traverse` | NASA/JPL MMGIS Perseverance feed | traverse and waypoints | |
| `nomenclature` | IAU / USGS Gazetteer | `poi.json` | |
| `hirise_dtm` | USGS Mars 2020 TRN **HiRISE DTM** (1 m, 1.8 GB) | coverage, P90 slope, roughness | Sampled: three 1 m rows per 20 m row |
| `meda` | **Perseverance MEDA** (PDS Atmospheres) | `meda.json` | Sampled: 48 byte-range windows per file |
| `rover_images` | NASA/JPL Mars 2020 raw-image feed | index + 320 px thumbnails | Sols 1204–1400, five cameras; no key |
| `sharad` | **SHARAD** RDR list, PDS ODE | `sharad.geojson` | Tracks clipped to the region, with archive links; no data files fetched |
| `mcs` | **MCS** DDR V6.2, PDS Atmospheres | `mcs.json` | Multi-range reads of each profile's 1 kB surface line, not the 7–13 MB files |
| `hirise_ref` | **HiRISE** DTEEC DTMs, PDS | `hirise_ref_cov.bin`, `hirise_ref_slope.bin` | Gale only; kept apart from the terrain the planner reads |
| `manifest` | — | `manifest.json`, `regions.json` | Datasets, layers, presets |

`build_globe.py` writes `public/data/mars/globe/`: `color-8k.webp` / `color-4k.webp` (Viking MDIM 2.1, area-averaged), `relief-4k.webp` (MOLA east/north gradients, `128 + 846.67 × dz/dx`), `landmarks.json` (219 IAU features + the Perseverance landing) and `meta.json`. Check: MOLA read back as −8,160 m (Hellas) to +21,156 m (Olympus Mons).

**Adding a region**: copy `scripts/regions/jezero.json` to `scripts/regions/<id>.json`, edit its sources, run `build_region.py <id>`, and open `/planner/?region=<id>` (or pick it in the planner's selector). Steps without sources are marked unavailable and the app degrades gracefully. For a quick look anywhere, a globe site needs no build at all.

---

## Third-party 3D models (audit)

`public/models/external/manifest.json` records every third-party model considered (12 Sketchfab links plus one from an earlier list). For each it records:
- title, author, licence and downloadability, read from the official Sketchfab Data API (checked twice on 2026-10-16, unchanged);
- a status: integrated and verified, file present but not loaded, referenced but missing locally, download unavailable, or licence review;
- the credit line, any placement basis, and the manual action.

Downloads need a signed-in Sketchfab account, so **none is downloaded and none is integrated**.

`src/lib/externalAssets.ts` places a model in a region's 3D view only when **all** of these hold:
- its file is present;
- four checks are marked done: licence, geographic position, orientation, scale;
- heading and scale are set.

A placed model is credited in the 3D header. Nothing is placed at random. `node scripts/inspect_glb.mjs <file.glb>` prints a model's nodes and its bounding box in model units, with transforms applied and quantized meshes decoded. It reads NASA's Perseverance model as 2.705 × 2.232 m, the rover's real width and height. It also reports any node named like a scale bar.

**To add *Rose River Falls, Sol 449*** (CC BY 4.0, the Mastcam-Z team; the best candidate, because its position is Perseverance's own sol-449 localization and it carries a 1 m north-pointing scale bar):
1. Sign in at sketchfab.com and open the model's page.
2. *Download 3D Model* → the autoconverted **glTF Binary (.glb)**, saved as `public/models/external/rose-river-falls-sol449.glb`.
3. Run the inspector on the file.
4. In the manifest, set `placement.scale` (= 1 ÷ the bar's length in model units) and `placement.headingDeg` (the bar's direction, clockwise seen from above, from the model's −Z axis).
5. Refine `lat`/`lon`. They are the rover's end-of-drive position, and the outcrop is within the arm's reach in front of it.
6. Set the four `verification` flags, check the model in the Jezero 3D view, and only then set its status to `integrated`.

Other models:
- **Downloads disabled:** Kimberley, the Gale collection, Tharsis Facility and the communications tower.
- **Paid Store asset:** the modular base.
- **Licence review:** "Perseverance MastCam-Z" is ground photogrammetry under CC BY-NC, not a rover model, so MOMAR keeps NASA's Perseverance.

---

## Verification

- **SOZO integration** (2026-10-30, the production build under `next start` with **no SOZO environment variables**, against a local stand-in for SOZO's companion API and a stand-in OpenAI-compatible server on Ollama's port; headless Chrome at 1470 px and 390 px; no console errors): the chat opened from the floating SOZO (a small *Connect SOZO* button before a key, no 3D) on the built-in engine, which answered with its labelled lines as before; Settings showed SOZO on top, Claude and OpenAI side by side, then Local model and Built-in; a pasted key was checked with SOZO and kept in a cookie scripts cannot read (`document.cookie` does not show it); SOZO then appeared in the chat in 3D, the *Show SOZO in 3D* switch removed and restored it, the top sensor scanned (top to bottom, then side to side) only while a question was being answered, and the floating SOZO turned into the 3D companion; on a Jezero scenario *Compare the two routes* reached SOZO with the short-answer request, 400 lon/lat points, MOMAR's figures (3.93 km, 2.70 h, 19.3°, mean 10.5°, 20 m), the other route and 24 planner facts, with no `Origin` and no cookies, while *Explain this route* went as typed; the answer showed SOZO's comparison table, source and limitations; **▶ Listen** played SOZO's voice and SOZO showed *Speaking*; *Find local models* found the stand-in Ollama with 2 models, whose streamed answer hid its `<think>` preamble and flagged a number not in MOMAR's data; **SOZO Chat** at the top of the 3D playback opened the same conversation on the right (Mars and Moon); on the Moon the route went as lon/lat from 138°W 89.45°S with 7.66 km, 1.85 h, 14.7°, 240 m and the sunlit and Earth-in-sight shares, and the conversation followed the chat into the 3D view; the SOZO page's *Launch* opened the Mars planner with the question filled in, sent nothing, and cleared the address; the navbar reads Vision · SOZO · How it works; phones show no horizontal scroll and a tap on SOZO opens the chat. Then SOZO on the map: a click on SOZO before a key opened the chat on SOZO's settings; once connected SOZO stood on the map; a click opened the question box at the bottom centre; *Compare the two routes* there was answered in place and the same exchange was in SOZO Chat; *Observe the terrain along this route* sent SOZO through three stops across the map (left, further left, then up), scanning in a different direction at each, and back to its place before the answer appeared (7.5 s with a 2.5 s stand-in answer), while *How long is the walk?* did not move it; in the Mars and Moon 3D playback SOZO stood on top in the bottom-right corner and answered through its question box; a stand-in `provider_unreachable` failure showed the explained message with *Try again*; folding the side panel widened the map from 1,050 to 1,410 px and its *Layers* symbol reopened it on Layers; the Moon behaved the same; the company page shows no "Siwom" brand name. Not exercised: real SOZO answers (no key was available to this build), real OpenAI calls, and a local model reached from https://www.momar.si (browsers may ask permission to reach apps on this device).

**Data integration** (2026-11-03): `npm test`, `npm run typecheck`, `npm run lint` and `npm run build` on the production build, then headless Chrome on the running site with no console errors. New tests: `tests/moon/evidence.test.ts` (LEND, Mini-RF, LAMP, CRaTER against independent data, and routes identical with and without them), `tests/marsReference.test.ts` (SHARAD, MCS, HiRISE at Gale, kept out of route risk), `tests/spice.test.ts` (SPICE against Horizons and MOMAR's sky code, with stated tolerances), `tests/dataEcosystem.test.ts`, `tests/readmeNumbers.test.ts`. In the browser: all three lunar layers line up with their craters; SHARAD popups open and their archive links respond; the CRaTER and MCS panels read their tables; Gale's "Named features" card no longer crashes (a mission object in its source card, fixed at the source and guarded by a test). A few heavy tests can pass the 5 s limit on a heavily loaded machine; they pass when it is free, or with `--testTimeout`.

Checked earlier (2026-10-26): typecheck, lint, **432 unit tests** (327 Mars, 105 Moon; **460** with the SOZO tests on 2026-10-30), and the static build (routes `/`, `/explore`, `/globe`, `/globe/site`, `/planner`, `/how-it-works`, `/vision`, `/contact`, `/company`, `/moon`, `/moon/south-pole`, `/moon/missions`). Browser checks in Chrome:
- **Reliability sprint** (headless Chrome over the DevTools protocol, 1470 × 900, Metal GPU, tab visible, vsync-capped at 60 Hz): Mars 3D rover, astronaut and both playing at 60 fps (worst frame 16.8 ms; 186–198 fps with the cap off); globe 60 fps; Gale planner ready in 0.8 s and its 3D at 60 fps; Moon 3D at 60× held 60 fps over the whole 7.6 km traverse after the fix (before: 43.6 fps with 150–180 ms hitches from rebuilding the close range and the 5 m patch); every page loads without console errors; at 390 px no page scrolls sideways; with the tile services and the USGS archive blocked the globe falls back to its base map, a globe site says the archive is unreachable, and the Jezero planner works. Thermal readouts checked at Jezero (THEMIS 227, TES 257 TIU) and a Gale site (TES ≈ 338). The lunar report prints to PDF with its dark colours.
- **Mars upgrade** (Chrome, 1470 px): the Departure & sunlight panel at 09:00 LTST on Perseverance sol 2020 (Sun 44° up, matching a hand calculation of 44.2°); the terrain-shadow layer at a 2° Sun from the east; the 3D Sun at 06:00, noon and night (the scene darkens after sunset); Look at → Earth (14° up, 1.47 AU on 2026-10-27); CRISM observations at stop S1 (30, live from ODE); the globe at 3× and 10× relief (Tharsis and Valles Marineris in profile, no cracks, no console errors). Moon: the 3D camera held above the ground when dragged below the horizon; the Date control stepping the traverse start. Animation smoothness was not judged (the automated tab may be hidden).
- **Globe** at 8,151 km, 1,800 km, 300 km, 150 km, 40 km, 8 km and 3 km: Jezero, Gale, Olympus Mons, both poles and the ±180° seam. No failed requests, no console errors. Olympus Mons reads 20,012 m.
- **Sites** at Jezero, Gale and 68°N. The Jezero landing site reads −2,573 m on the 200 m DEM (the published value is −2,570 m). At 68°N, outside the night-IR coverage, the panel says so.
- **Tile service blocked**: the globe, site map and 3D view each fall back and say so.
- **Jezero regression**: the 2D planner and 3D playback still work, at about 100–200 fps uncapped (rover alone ≈ 140, rover + astronaut ≈ 100).
- **Phones**: the globe, landing page and site planner at 390 px wide, with no horizontal scroll.
- **Rovers**: both models load, roll and keep every wheel on the ground (figures above); route smoothing shown at a Gale site with the grid path overlaid.
- **Moon** (headless Chrome, 1440 px and 390 px, no console errors): the south-pole workspace loads LOLA grids and the Horizons table, streams NAC imagery and 5 m slopes, inspects a ridge site at 89.45°S 137°W (Sun 1.77° up, 4.33° clear of the terrain; Earth sets behind terrain 2026-11-03 21:22 UTC), runs a 30-day timeline, a sunlight map (32 % of the area sunlit, 6.7 s), a 25 km three-point route with light at arrival and a return check, a two-site comparison, the Scientist and the 3D view (the Sun's azimuth advancing 0.5° per simulated hour during playback). The globe reads Apollo 11's LOLA elevation (−1,927 m; LROC −1,926 m). The Mars planner, Mars globe and planet picker were re-checked after the Moon work.
- **Lunar planner** (Chrome, 1470 px and a 390 px frame, no console errors):
  - **Routing:** A at 89.45°S 137°W and B at 89.25°S 112°W plan automatically, 10.5 km, compared side by side; the farthest point (10.4 km from A) is flagged beyond the Apollo record of 7.6 km. "Add as stop" on the nearest Diviner cold trap re-plans A → S1 → B.
  - **Map and tabs:** the hover readout gives elevation, slope, long-term Sun and Earth, Diviner temperature and ice depth. The Images tab lists 17 of 60 indexed NAC images covering a point, with footprint and browse strip. The long-term sunlight, cold-trap and other new layers draw correctly.
  - **Chat:** the built-in Scientist answers with labelled lines (the mission report is covered by unit tests, not opened in the browser).
  - **3D:** the LRV drives with rolling, steering wheels and stands tilted on a 10° slope at B; at a stop the crew walks out (HUD: lope at 0.80 m/s, Froude 0.41, 0.46 s flights, 4.3 cm rise at g = 1.62 m/s²). A walking EVA and a VIPER-class route (generic rover, long low-Sun shadow) both run; crew and Sun & Earth toggles work. Playback speed could not be judged: the automated tab was hidden, so Chrome paused animation frames.
  - **Close-range patch:** building its 263,169 points takes 22 ms of coordinate conversion (measured in Node).
  - **3D mission playback** (full screen): the LRV on a 3.1 km route driving down a 17.6° slope (pitch −17°, wheel tracks behind it, rocks with long low-Sun shadows kept off the track); the inspector at mid-route reading 1,628 m on the 5 m DEM, 18.7° on the 240 m cell, long-term sunlight 37.9 %, Diviner 281 K; at B the crew lopes out from the LRV to sample ("At B · crew sampling, 22 min left"); Astronaut mode walking the route at 0.8 m/s (2.2 h with stops; Froude 0.41, 0.46 s flights); Look at Earth shows a nearly new Earth beside the low Sun. Dust grains are drawn but were hard to judge in still frames, and playback smoothness could not be judged (the automated tab was hidden).

---

## Limitations

- **Reference layers are not evidence of ice or of what lies below.** LEND's map is smoothed to about 15 km, Mini-RF's CPR cannot separate rough ground from ice, LAMP's albedo is noisy and can reflect porosity, SHARAD's tracks come with no interpretation, MCS is a coarse archived climate at the orbiter's fixed local times, and CRaTER measured silicon dose in lunar orbit, not on the ground. None of them changes a route or a risk score.
- **Orbital terrain only.** Jezero's 20 m DTM misses boulders, small scarps and ripples, and its HiRISE 1 m data are sampled over about a third of the region. Globe sites use 200 m posts: slopes and routes there are regional estimates. Smoothing rounds the grid's corners within a 1 % cost budget; where that budget is not enough, a kink stays.
- **Thermal inertia**: THEMIS-derived values are about ±20 % and its mosaic has image seams; TES pixels are about 3 km, and the site tiles store them in 8.4 % steps. Neither changes route risk.
- **Gale**: the 20 m grid samples one 1 m row per cell (a transect mean); no geologic map, HiRISE fine slopes, weather or rover photographs are integrated there.
- **Tobler's function** describes Earth hikers, not suited astronauts in 0.38 g. The second estimate shown beside it (gravity-scaled Minetti) is also not suit data: it shows how much the answer depends on the model.
- **Theta\*** is not optimal; the planner keeps whichever of it and the A\* route is cheaper. Its vertices stay within 16 cells (320 m in Jezero) of the A\* path, so it refines that route rather than exploring every alternative.
- **Sun and shadows** are for the Sun's centre on the DEM's own relief: boulders, small scarps and terrain outside the region cast no shadow, and dust in the air is not modelled. Earth's position in the Mars sky is good to a fraction of a degree (approximate planetary elements, no light time).
- **CRISM** parameters show absorptions consistent with mineral classes, not identifications. The geologic map is generalized at 1:75k and predates rover-era findings.
- **Surface classes** at sites are relative night-time infrared plus albedo: a proxy, not calibrated thermal inertia. They are only available between 60°S and 60°N. Sites stop at ±80° latitude.
- **Imagery colour** is the Viking mosaic's (approximate, graded for display), with brightness from CTX. Straight edges at mid zoom near the poles are frame boundaries inside the Viking mosaic itself (patch outlines show they do not follow the tile grid). The CTX mosaic has its own image seams. The finest imagery is ~5 m/px, so it cannot get sharper below a few kilometres up.
- **Live services**: sharp globe imagery needs the OnMars tile services, and sites need the USGS archive. Without them MOMAR falls back and says so. HiRISE, THEMIS and MOLA context tiles come from trek.nasa.gov.
- **3D is illustrative**: rocks, outcrops, fine relief, gravel, slabs, strata, sand, dust, wheel tracks, weather and lighting are procedural, not observations or hazards.
  - The rover body sits on a plane fitted to its six contacts and each wheel moves vertically to meet the ground; the rocker and bogie arms do not articulate, and there is no slip, sinkage or corner steering (not yet built).
  - 3D close-ups are soft because the colour layer is ~5 m CTX. HiRISE tiles are served without CORS headers, so WebGL cannot read them.
- **Astronaut**: the gait is adapted with documented but approximate factors; it is not Mars-gravity or spacesuit biomechanics. Its height (1.90 m) is assumed.
- **Rover photographs** are matched by the rover's recorded position, not by what they show.
- **SOZO Chat**: the built-in engine matches by rules, so unusual wording may not map. SOZO needs your own SOZO key (it has no free tier inside MOMAR unless the host adds one). Answers from SOZO, Claude, OpenAI and local models are model-written: numbers are checked against MOMAR's data (SOZO's against its own comparison table), wording is not. A Claude or OpenAI key used in the browser is visible to the page and extensions, so use one with a spending limit. Local models need CORS allowed for this site, and small ones may misread the data.

## What to improve next

SOZO: voice questions (SOZO's speech-to-text), a shared-proxy rate limit that holds across serverless instances (a KV store), MOMAR accounts so the proxy can require sign-in, SOZO research reports (`/api/v1/research`) for longer questions, and sharing a map screenshot with an explicit preview. On SOZO's side, `/api/companion/model` labels its already-decoded body `Content-Encoding: br`, so browsers reject it; MOMAR loads `/models/sozo-companion.glb` instead (removing that header in SOZO's handler fixes the endpoint).

Done in this round: any-angle routing, validation against Perseverance's drives, Mars time and Sun position, terrain shadows and sunlight along routes, Earth in the Mars sky, globe relief in profile, CRISM observations on demand, a second walking-time model, layer search. Still open (see `docs/mars-upgrade-prompt.md` for the audit):

Done since: thermal inertia (THEMIS and TES, for regions and every globe site), Gale Crater as a second detailed region, route data basis with no obstacle claims, smooth Moon 3D playback, the dark lunar report. Still open (see `docs/mars-upgrade-prompt.md` and the sprint report `docs/mars-sprint-report.md`):

1. **Obstacle data**: a georeferenced boulder catalogue (automated HiRISE detection) and a finer cost grid near the route; until then routes avoid only what 20 m models resolve.
2. **Gale extras**: Curiosity's raw images (a different feed), REMS weather, a geologic map, and validating the separate HiRISE DTMs (now a reference layer) against Curiosity's drives before route risk uses them.
3. **HiRISE imagery cached at build time** for 25 cm 3D close-ups (the tiles have no CORS headers; the Gale 25 cm mosaic is 23 GB without overviews).
4. **Rover mechanics**: articulated rocker and bogie, corner steering, simulated slip and sinkage, labelled as simulation; a Curiosity model for Gale.
5. CRISM spectra for a stop (reading the archive cubes), and suit-mobility data instead of Earth-derived walking models.

## Repository map

```
scripts/build_region.py, scripts/pipeline/*.py, scripts/regions/jezero.json   Jezero data pipeline
scripts/build_globe.py                                                         globe base maps and landmarks
scripts/inspect_glb.mjs                                                        inspect a downloaded 3D model
scripts/moon/evidence.py, scripts/pipeline/{sharad,mcs,hirise_ref}.py          orbital evidence and reference data
scripts/build_spice.py, tests/spice.test.ts, tests/fixtures/spice/             NAIF SPICE cross-check of the Sun and Earth
src/lib/data/{ecosystem,numbers}.ts                                            the public data list and the README's numbers, both read from the build
public/data/jezero/, public/data/mars/globe/                                   processed static data
src/lib/{grid,terrain,cost,pathfind,anyAngle,route,traverse,itinerary,science,mission,compare,render}.ts   planning logic
src/lib/{marsTime,marsShadow,marsSky,walkModels,traverseValidation,crismOde,globeRelief}.ts  Mars clock and Sun, shadows, Earth in the sky, walking models, rover-drive comparison, CRISM index, globe relief
src/lib/{thermal,tesTiles}.ts                                                  thermal-inertia classes and rendering; TES tiles for globe sites
scripts/regions/gale.json, scripts/pipeline/thermal.py, scripts/build_tes.py   Gale region, thermal-inertia step, TES tiles
src/lib/{globe,globalData,site,siteRegion,marsTiles,vocab}.ts                  globe, live archive reads, globe sites
src/lib/{playback,walk,rocks,terrainDetail,externalAssets}.ts                  3D geometry and asset rules
src/workers/planner.worker.ts                                                  slope, risk, Theta*/A*, shadows, sunlight, overlays off the main thread
src/components/globe/*, src/app/globe/, src/app/globe/site/                    globe and site planner pages
src/components/planner/*                                                       map, panels, report, chat, 3D, weather
src/components/home/*, src/app/(site)/*                                        landing page, /sozo/ page
src/lib/sozo/*, src/app/api/sozo/*                                             SOZO: context mapping, server proxy (keys, limits, retries), browser client
src/components/sozo/*                                                          SOZO 3D companion, floating dock, chat parts and settings
docs/sozo/                                                                     SOZO's README and companion API guide (copies)
public/models/                                                                 planet, rover, astronaut and LRV models; asset audit
tests/*.test.ts                                                                Vitest
scripts/build_moon.py, scripts/moon/*.py                                       lunar data pipeline (Horizons, PGDA, USGS, LROC, Moon Trek)
public/data/moon/                                                              lunar ephemeris, south-pole grids, globe, composition, geology, names, missions
src/lib/moon/*.ts, src/lib/geotiff/cog.ts                                      lunar frames, ephemeris, sky, horizons, illumination, analysis, catalogue, scientist, report; COG reader
src/components/moon/*, src/workers/moon.worker.ts, src/app/moon/               Moon globe, south-pole planner (map, tabs, chat, 3D), mission catalogue
src/lib/moon/{mobility,traverse,lunarGait,lrvModel,scientistAI}.ts             mobility presets, traverse timeline, lunar gait, LRV rig, Claude mode
src/lib/moon/{regolith,dust,ode}.ts                                            illustrative craters and rocks, ballistic dust, LROC image index
tests/moon/*.test.ts, tests/fixtures/moon/                                     lunar tests and their independent references (Horizons, PROJ, GDAL, ODE)
docs/moon-planner-prompt.md                                                    review of the lunar planner prompt and an improved version
docs/mars-upgrade-prompt.md                                                    review of the Mars upgrade prompt, what was done, and an improved version
```

## Credits & licences

**Data**: NASA/JPL-Caltech, USGS Astrogeology Science Center, MSSS (CTX), The Murray Lab/Caltech (CTX global mosaic), ESA/DLR/FU Berlin (HRSC, in the blended DEM), NASA GSFC (MOLA), University of Arizona (HiRISE), JHU-APL (CRISM), ASU (THEMIS), CAB/INTA-CSIC (MEDA), PDS Geosciences and Atmospheres Nodes, IAU WGPSN; tiles via Esri OnMars and NASA Solar System Treks. Citations for every dataset appear in each layer's info card and in the manifests.

- **CTX global mosaic**: "The Murray Lab/Caltech grants free use of the V01 version of the mosaic for all purposes. Data credit should be to: NASA/JPL/MSSS/The Murray Lab". Cite Dickson, J. L., Ehlmann, B. L., Kerber, L., & Fassett, C. I. (2024), *The Global Context Camera (CTX) Mosaic of Mars: A Product of Information-Preserving Image Data Processing*, Earth and Space Science 11(7), [doi:10.1029/2024EA003555](https://doi.org/10.1029/2024EA003555).
- **HRSC + MOLA blended DEM** (USGS Astrogeology, 2018): its use constraint is to cite the originators (DLR; NASA GSFC); HRSC data ESA/DLR/FU Berlin.
- **Esri OnMars** map services state only their copyright lines ("Source: NASA/JPL/MSSS/Caltech Murray Lab/Esri", "Source: NASA/JPL/USGS/Esri"). For a production release, confirm Esri's terms for those endpoints or serve the downloadable mosaics yourself.
- **Perseverance 3D model**: "Mars Perseverance Rover, 3D Model", NASA/JPL-Caltech, from [science.nasa.gov](https://science.nasa.gov/resource/mars-perseverance-rover-3d-model/), under the NASA Images and Media Usage Guidelines.
- **Spirit & Opportunity 3D model**: "Spirit and Opportunity Rover, 3D Model", NASA/JPL-Caltech, from [science.nasa.gov](https://science.nasa.gov/resource/spirit-and-opportunity-rover-3d-model/), under the NASA Images and Media Usage Guidelines.
- **Apollo Lunar Roving Vehicle 3D model**: "Mondfahrzeug (Lunar Rover)" by Deutsches Museum | Digital (Inv.-Nr. 2019-407), from [Sketchfab](https://sketchfab.com/3d-models/mondfahrzeug-lunar-rover-acb3f8645a69493cb211a62b40d66aab), [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/); optimised and rigged by MOMAR, and the adapted file is shared under the same licence (`public/models/lrv/SOURCE.md`).
- **Astronaut 3D model**: "Animated Astronaut Character in Space Suit Loop" by LasquetiSpice, from [Sketchfab](https://sketchfab.com/3d-models/animated-astronaut-character-in-space-suit-loop-8fe5c8d3365e4d87bb7bc253d53a64e1), [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/); converted and compressed by MOMAR (`public/models/astronaut/SOURCE.md`).
- **Perseverance raw images**: credited per image as published (NASA/JPL-Caltech; NASA/JPL-Caltech/ASU for Mastcam-Z; NASA/JPL-Caltech/LANL/CNES/IRAP for SuperCam RMI), under the [JPL Image Use Policy](https://www.jpl.nasa.gov/jpl-image-use-policy/) and [NASA Images and Media Usage Guidelines](https://www.nasa.gov/nasa-brand-center/images-and-media/).
- **Landing page**: the hero video and the robotics illustrations are supplied concept artwork, labelled as such.
- **Third-party Sketchfab models** are not used (see [the audit](#third-party-3d-models-audit)); the CC BY-NC one stays disabled pending a decision on its non-commercial terms.

**Moon**: NASA/GSFC Planetary Geodesy Data Archive (LOLA polar products, Barker et al. 2021, 2023), NASA/GSFC/Arizona State University (LROC NAC and WAC), NASA/KARI/ASU (ShadowCam), USGS Astrogeology (mosaics, LMAP cloud-optimised products, Ice Favorability Index, Unified Geologic Map — Fortezzo, Spudis & Harrel 2020), JAXA/SELENE (Kaguya Multiband Imager; Lemelin et al.), IAU WGPSN (names), LROC team (Wagner et al. 2017, human-made objects), NASA/JPL Solar System Dynamics (Horizons, DE441), NASA Solar System Treks (WAC tiles), LOLA team (long-term visibility, Mazarico et al. 2011), Diviner team NASA/JPL/UCLA (Polar Resource Product, Paige et al. 2010), PDS Geosciences Node (Orbital Data Explorer). Mobility values: NASA NSSDCA (Apollo LRV), Jacob et al. 2026 (LPSC), NASA VIPER documents, Carr & McGee 2009.

**Orbital evidence and reference data**: NASA/IKI (LRO LEND; Mitrofanov et al. 2010, Science 330, 483), NASA/JHU-APL (LRO Mini-RF; Nozette et al. 2010, Space Sci. Rev. 150, 285), NASA/SwRI (LRO LAMP; Gladstone et al. 2012, JGR 117, E00H04; doi:10.17189/1520244), NASA/University of New Hampshire (LRO CRaTER; Spence et al. 2010, Space Sci. Rev. 150, 243), ASI/NASA (MRO SHARAD; Seu et al. 2007, JGR 112, E05S05), NASA/JPL (MRO MCS; McCleese et al. 2007, JGR 112, E05S06; McCleese & Schofield, DDR V6.2, doi:10.17189/6khc-s052), NASA/JPL/University of Arizona (HiRISE DTMs; McEwen et al. 2007; Kirk et al. 2008), NASA/JPL NAIF (SPICE toolkit and generic kernels; Acton 1996, Planet. Space Sci. 44, 65), all from NASA's Planetary Data System and the instrument teams' public archives.

MOMAR is not affiliated with or endorsed by NASA.
