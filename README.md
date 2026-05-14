# DMA Selector

An interactive map for selecting Nielsen Designated Market Areas (DMAs). Built with [Leaflet](https://leafletjs.com/) and served as a static page via GitHub Pages.

Designed to be embedded in an `<iframe>` and communicate selections to the parent application via `postMessage`.

**Live:** https://ahmdatef.github.io/dma-selector/

---

## Features

- Click any DMA polygon to select / deselect it
- Hover tooltip showing DMA name, ID, and TV penetration %
- **Select All** — selects every DMA at once
- **Invert** — flips the current selection (selected ↔ unselected)
- **Clear** — deselects everything
- Named groups rendered in distinct colors, passed as query parameters
- Group panel (top-right) listing all groups; click a group to load its DMAs as the active selection

---

## Query Parameters

Pre-defined groups of DMAs can be passed as query parameters using the `group-<name>` convention. The value is a comma-separated list of DMA IDs.

```
https://ahmdatef.github.io/dma-selector/?group-west=803,866&group-east=501,504
```

Each group is automatically assigned a unique color. If a DMA belongs to multiple groups, it takes the color of the first group listed.

---

## postMessage Events

Every selection change emits a `message` event to the parent frame:

```js
window.addEventListener('message', function (event) {
  if (event.data.type === 'dmaSelectorUpdate') {
    console.log(event.data.selectedDMAs);   // number[]
    console.log(event.data.lastClickedDMA); // number | null
  }
});
```

| Field | Type | Description |
|---|---|---|
| `type` | `"dmaSelectorUpdate"` | Event identifier |
| `selectedDMAs` | `number[]` | All currently selected DMA IDs |
| `lastClickedDMA` | `number \| null` | The most recently toggled DMA, or `null` if selection is empty |

---

## Embedding

```html
<iframe
  src="https://ahmdatef.github.io/dma-selector/?group-northeast=501,803"
  width="100%"
  height="600"
  frameborder="0"
></iframe>
```

---

## Local Development

```bash
cd docs
python3 -m http.server 8080
# open http://localhost:8080/?group-west=803,866&group-east=501,504
```

No build step — everything is a single self-contained HTML file (`docs/index.html`).

---

## Data

DMA boundaries from Nielsen (`docs/nielsen-dmas.geojson`). Each feature includes:

| Property | Description |
|---|---|
| `dma` | DMA numeric ID |
| `name` | Market name |
| `tvperc` | TV penetration % |
| `cableperc` | Cable penetration % |
| `adsperc` | Advertising penetration % |

---

## Deployment

GitHub Pages serves from the `docs/` folder on the `main` branch.  
Settings → Pages → Source: `main` / `/docs`.
