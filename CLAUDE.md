# CLAUDE.md: Davis XC web portal

Public repo. Never add athlete data, names, CSV or JSON data files here. The data pipeline lives in the sibling folder
../pipeline (private, not in git; read its CLAUDE.md for the model and data). Copied in design from Kevin's Jesuit XC web
portal (../../jesuit-xc/web, read only, never change it); no shared code.

- templates/portal_template.html: the portal page, single source for the local build and the signed-in web build. The
  pipeline's scripts/build_portal.py replaces `__DATA__` with data/portal_data.json (from scripts/portal_data.py) and writes
  ../pipeline/portal/davis_xc_portal.html, which stays local. build.py writes dist/index.html with `window.D = null`
  and the app script as type text/x-jx-app; src/gate.js signs the member in, reads portals/xc, sets window.D and runs it.
  The school seal is embedded once in the template (base64 PNG, 112 px, from ../brand/dshs-seal.png); build.py reuses it
  in the gate header (`__SEAL_SRC__` in src/gate.html).
- Live 2026-11-07: https://kevinwelling12.github.io/davis-xc/ (repo kevinwelling12/davis-xc, public, Pages from GitHub
  Actions on push to main). Firebase project davis-xc: Google sign-in, Firestore (production mode) with rules from
  firestore.rules (paste into the console and Publish after editing), authorized domain kevinwelling12.github.io.
  src/firebase-config.js holds the public web config. Firestore: members/{email lowercase} {role: admin|coach, name,
  added, by}; portals/xc {version, chunks, bytes, as_of, updated}; portals/xc/chunks/NNN {data: gzipped JSON}.
  Coaches and parent volunteers both get the coach role (same full view). Gate: saved copy in IndexedDB 'davis-xc',
  member memo 'dx-member', retry flag 'dx-retry', 20 s watchdog, version stamp in the footer.
- Icons (src/static, copied to dist/ by build.py with manifest.webmanifest): the block D with devil from Kevin's design
  system (../brand/blue-devil-d.png): apple-touch-icon 180, icon-192, icon-512 (art at 92% on white), favicon-32. Use the
  supplied art only; never redraw or alter it.
- After any gate or rules change: a signed-out REST read of portals/xc must return 403, and the live page must show the
  sign-in card with no console errors.
- Look: Kevin's Davis High Athletics design system (https://claude.ai/artifact/EzqWX3acJ7teryG8VSyH11, project/tokens.json).
  Montserrat 500/700/800 (Google Fonts). Header band devil-blue #2D3289 with the seal on a white disc and a white
  wordmark; tabs and footer on devil-navy #142E54. Headings in sentence case (h2 28/34 brand-ink, h3 18/24 heading blue);
  tabs, chips and table heads 12 px 700 caps with 1 px tracking. Radius 4 px for controls, 8 px for cards; no left-border
  notes. Pressed chips devil-blue (light) or #A9AFFF with dark text (dark). Davis rows get a light devil-blue tint with a
  blue left edge. Tokens on :root, dark under prefers-color-scheme guarded by :root:not([data-theme="light"]) and again
  under :root[data-theme="dark"].
- Boys and girls are separate everywhere: a Boys/Girls chip pair at the top of every tab sets one shared choice (S.g,
  saved in localStorage 'dx-ui' with the tab, runner, meet view and championship). Clicking a runner or race switches to
  its gender.
- Tabs: Roster (rating = Willow Hills 5K today), Runner (chart of every race as a Willow Hills 5K on its day; history with
  the strict pre-meet expected time, shifted by how the rest of the field ran, and the 8-in-10 range), Meets (Report,
  Results, Team scores; races of the chosen gender), Projections (Section finals and State for every current runner of
  that gender, distinct places by projected time, with the measured field shift for that gender), Simulation
  (section_sim.py results; the Boys D1 or Girls D1 race by the chip; scenarios as chips), Courses (D.courses: each course and
  distance Davis has raced, per gender, as % faster or slower than the Willow Hills 5K after distance, and the time there for a
  Willow Hills 5K time the viewer types, starting at the middle of Davis's current top 7; 1-2 rated races marked few races), Model (strict backtest by gender
  from accuracy.by_g and by_year with g; distance exponent and growth per gender).
- Team scores are unofficial: teams of 5+, top 5 score, 6th and 7th displace, exhibition and unattached left out.
- Before any commit: run ../pipeline/scripts/check_web_clean.py, which searches this repo for every athlete name in the
  master and any data file. It must report clean. Never write a runner's name here, even as an example.
- Serve local test pages only on 127.0.0.1 from the scratchpad, never the pipeline's portal/ folder, never all interfaces.
- Kevin prefers plain, direct writing: no em dashes, no filler.
