# AI Campus Map (read-only demo)

**Student-built visual from the x402 “DormDAO / Sealevel Campus” project.**
A lightweight HTML5 canvas that renders a campus map, five walking agents, and a roaming director. Agents pick destinations, pathfind using A*, wait at doors, avoid narrow bridges with simple locks, and occasionally speak short quotes. No frameworks, no build step, just one HTML file.

> **NFA / Educational use.**
> There are no private keys, API tokens, or network calls in this repo.

---

## ✨ Features

* **Zero-dependency** single file (`index.html`) — works on any static host.
* **A*** pathfinding on a hand-authored **graph** of walkable nodes/edges.
* **Movement logic**: spawn separation, dwell at “entrance” nodes, basic “narrow-edge” locking to avoid overlaps on bridges, crowd-aware destination choice.
* **Configurable tuning** via constants (speed, dwell ranges, “prefer longer routes” bias).
* **Character system**: agents with names, images, home bases, destination weights, and quotes.
* **Canvas rendering**: shadows, nameplates, speech bubbles; scales with the map image size.

---

## 📸 Preview

```
/ (canvas 16:9)
  ├─ map.png                (campus background)
  ├─ ava.png, max.png, ...  (agent sprites)
  └─ index.html             (this demo)
```

*(Add a screenshot in this section once you deploy.)*

---

## 🚀 Quickstart

1. **Clone / download** this repo.
2. Put your image assets next to `index.html` (see “Assets” below).
3. Open `index.html` in a browser.

> For a tiny local server (avoids some browser file restrictions):
> `npx serve .` or `python3 -m http.server` and open [http://localhost:8000](http://localhost:8000)

---

## 🧩 File & Code Walkthrough

* **Assets block**

  ```js
  const ASSETS = {
    map:'map.png',
    ava:'ava.png', max:'max.png', noor:'noor.png',
    diego:'diego.png', hana:'hana.png', director:'director.png'
  };
  ```

  Replace file names to match your images.

* **Tuning**

  ```js
  const SPEED_MULT = 1;
  const EDGE_LOCK_MS = 1800;  // hold narrow bridge edges
  const FAR_BIAS    = 0.95;   // prefer longer routes
  const MIN_HOPS    = 5;      // aim for paths with at least N edges
  // Dwell ranges (ms)
  const JUNCTION_DWELL_MIN = 0,    JUNCTION_DWELL_MAX = 0;
  const ENTRANCE_DWELL_MIN = 1100, ENTRANCE_DWELL_MAX = 1600;
  ```

* **Graph (walkable network)**

  ```js
  const GRAPH = {
    NODES: { /* id:{x,y} in 0..1 coords */ },
    EDGES: [ ['DA','W1'], ... ],
    LOCKED_EDGES: [ ['LW','LE'], ['MW','ME'] ],
    PLACES: { dormA:'DA', dormB:'DB', cafe:'CF', labs:'LB', quad:'QD', bank:'BK' }
  };
  ```

  * `NODES` are normalized; they scale with the map image size.
  * `LOCKED_EDGES` mark narrow passages where only one agent may enter at a time.

* **Spawns**

  ```js
  const SPAWN_NODES = ['DA','DB','CF','ME','LE','L2','E33'];
  ```

* **Quotes**

  ```js
  const QUOTES = {
    ava:[ "Data first. Memes later.", ... ],
    max:[ "Speed is my edge.", ... ],
    ...
  };
  ```

* **Agents**

  ```js
  new Agent({
    id:'ava', name:'Ava "Scholar" Li', imgKey:'ava', home:'dormB',
    weights:{cafe:2, labs:2, quad:2, bank:1, dormB:3},
    speed:.14, scale:.23
  })
  ```

  * `weights` guide destination choice (higher = more likely).
  * `home` must map to a key in `GRAPH.PLACES`.
  * `speed` is “map widths per second”; `scale` controls sprite height vs canvas.

* **Pathfinding (A*)**
  The `aStar(start, goal, nodes, edges)` returns a node path; distance uses Euclidean metric on normalized coordinates.

* **Director**
  A simple “patrol” between two points with a speech bubble.

---

## 🛠️ Customization

* **Change the background map**
  Replace `map.png` and keep the same aspect ratio, or set a new canvas size by changing `W/H` after images load.

* **Add / remove agents**
  Duplicate an entry in `AGENTS`, add a `png`, and (optionally) a quote list in `QUOTES`.

* **Edit walkable routes**
  Move `NODES`, connect with `EDGES`, and mark any narrow passages in `LOCKED_EDGES`.

* **Text / styling**
  Tweak fonts, colors, and bubble dimensions in the render functions.

---

## 🌐 Hosting

Any static host works:

* **GitHub Pages**: push to `main`, enable Pages → “Deploy from branch”.
* **Netlify** / **Vercel** / **Cloudflare Pages**: drop the folder; no build needed.

> If you later turn this into a SPA with deep links, add a history-fallback (`/* → /index.html`) to avoid 404s on nested routes.

---

## 🔒 Security & Privacy

* This demo contains **no secrets**, wallets, or API keys.
* No network requests; all assets are local images.
* Add a `.gitignore` for design sources if you keep raw files here:

  ```
  *.psd
  *.ai
  assets/originals/**
  .env
  ```

---

## 🖼️ Assets & Licensing

* Place `map.png` and character images (`ava.png`, `max.png`, `noor.png`, `diego.png`, `hana.png`, `director.png`) next to `index.html`.
* You are responsible for asset rights.
  Add an `ASSETS_LICENSE.md` describing authorship and licenses (recommended).

---

## 📈 Roadmap (nice-to-have ideas)

* Clickable characters → open public chat links (e.g., `x402scan.com/composer/agent/<uuid>/chat`).
* Seedable RNG for reproducible runs.
* JSON import for graphs/maps (export from a level editor).
* Lightweight UI toggles (speed, number of agents, labels on/off).
* Perf: offscreen canvas for large maps; dynamic sprite LOD.

---

## ❓ FAQ

**Q: Can I change the canvas size?**
Yes. The canvas uses the map image’s native size; you can clamp it or change `W/H` after loading.

**Q: Agents overlap sometimes — is that a bug?**
The demo intentionally keeps logic simple. Narrow bridges are “locked”; other intersections allow brief overlap. You can add local avoidance if needed.

**Q: Can I make agents speak custom lines?**
Add to the `QUOTES` arrays. Agents pick a random line during brief dwells.

---

## 🧑‍💻 Contributing

* Issues and PRs welcome (style: small, focused changes).
* Please avoid adding external libraries unless strictly necessary.
* If you add features, include a short comment block and keep defaults backward-compatible.

---

## 📜 License

* **Code**: MIT (or switch to Apache-2.0 if you prefer).
* **Images**: licensed separately — see `ASSETS_LICENSE.md`.

---

## ✅ Trust Checklist (for your project website)

* Link this repo as **“Map (read-only) source code.”**
* Pin this repo and your agents/paykit repos in your GitHub org.
* Add a short “Built in public. NFA.” disclaimer near the link.

---

### Copy-paste README badge suggestions

```md
![status](https://img.shields.io/badge/status-iterating-blue)
![license](https://img.shields.io/badge/license-MIT-green)
```

---

