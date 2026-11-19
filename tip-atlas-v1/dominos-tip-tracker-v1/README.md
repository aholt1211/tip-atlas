# Tip Atlas — Domino's delivery tip map (v1.0 prototype)

A phone-first, **static PWA**. No backend, login, paid hosting, or Mac required. Imports one or more screenshots of the Domino's **Tips → All** screen, recognizes rows on-device, allows manual correction, saves individual records in local browser storage, and draws geographic averages on a Leaflet/OpenStreetMap map.

## Install on iPhone (recommended)

1. Unzip this download on your Windows PC.
2. On GitHub, create a **public repository** (e.g. `tip-atlas`). **Upload the app's code files**, not customer screenshots or exports. Upload the **contents** of this folder to the repo root, not the enclosing folder.
3. In the repo open **Settings → Pages → Build and deployment**, select **Deploy from a branch**, `main`, `/ (root)`, and Save.
4. When GitHub Pages publishes your site, open `https://YOUR_USERNAME.github.io/tip-atlas/` in **Safari** on the iPhone. Use Share → **Add to Home Screen**.
5. Open Tip Atlas and select **Import**. Choose a screenshot of the Tips list. Review amounts and **Save approved**.
6. In **Settings**, set the correct shift date and your store's city/state before each import. Import overlaps are deduplicated using **shift date + order number**.
7. To draw your real delivery border, press **Start tracing** under Settings. Tap the map along your actual store-area outline, then go back to Settings and press **Save boundary**.
8. Map averages require coordinates. To demo, opt in to a **maximum of ten street-address lookups** through public Nominatim under Settings. These street addresses are sent to that geocoding service. Do not use this for large batches or if work rules prohibit it.
9. Export **Backup JSON** regularly (Settings), especially before changing phones or clearing browser/site data.

**Security & privacy:** Screenshots are processed inside the user's browser by Tesseract.js and are not retained or uploaded to the web host. Delivery rows and optional geocoded coordinates are saved in device-local browser storage only. Google/GitHub do not receive the imported screenshots through this app. OpenStreetMap tile requests are made when rendering the map; the demo geocoder receives exact street queries if explicitly activated. This is a personal prototype, **not affiliated with or endorsed by Domino's**. Do not upload screenshot data to GitHub.

**Limitations:** iPhone cannot silently read Domino's Delivery Experience app. OCR may misread prices; review before saving, particularly values above $30. OCR model/library and maps need internet initially. The app stores data locally in Safari/PWA storage and does *not* sync across devices. Map tiles depend on internet connectivity. No true production-grade batch geocoding is supplied. The real delivery boundary cannot reliably be inferred from the photo; it must be traced. Do not assume the example San Marcos map center is your actual store location. Public GitHub Pages exposes only app code (no local delivery records) but is not a private web host.

## Test on Windows first

From the unzipped folder run:

```powershell
py -m http.server 8000
```

Open `http://localhost:8000` in a browser and select Import → screenshots. Tesseract and Leaflet libraries load from CDNs. *For iPhone testing*, use the GitHub Pages HTTPS URL; service workers/PWAs require HTTPS except localhost.

## Known test case

The supplied sample screenshot had visible rows beginning with order 115, 127, 144, 166, 210, etc. In a local Tesseract command-line test, the first $7.13 row was recognized as `$713` (missing decimal). This implementation flags very large values for review, but cannot guarantee a correct OCR result. The Python command-line OCR test is not the same engine/configuration as the actual browser OCR; verify its output on your iPhone.

## Features

- iPhone-friendly PWA, 4 tabs
- Multi-image screenshot import and Tesseract.js browser OCR
- Per-row review/edit/check and suspicious-amount warnings
- Shift-date/order-number deduplication
- Local storage, CSV export, JSON backup/restore
- Interactive map and editable delivery-boundary polygon
- Geographic cell heat map with configurable minimum delivery sample size
- Explicit opt-in, small-scale demo geocoding; no automatic transfer of addresses

## Future upgrades

Installable native share-sheet ingestion via Shortcuts, safer large-scale location matching, high-quality row-level OCR layout processing, IndexedDB migration, richer statistics, exports from the Domino's system where allowed, optional encrypted sync, and time-of-day trends if that information becomes available.
