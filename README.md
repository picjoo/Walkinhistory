# Tehran 1968 — Historic Map

An interactive historic map. A visitor opens the page, taps **Find me**, and sees their GPS
position on the map with the historic buildings and places around them — each with a popup
showing its year, category, description and photo. As the admin, you unlock a control panel
to add, edit and delete points, then export a `data.json` you commit back to the repo so
every visitor sees the updated map.

Built with **only free resources**: HTML/JS, [Leaflet.js](https://leafletjs.com),
[OpenStreetMap](https://www.openstreetmap.org) tiles, and [GitHub Pages](https://pages.github.com)
for hosting. No server, no database, no build step.

---

## Files

| File | Purpose |
| --- | --- |
| `index.html` | The whole app — UI, map, geolocation, admin panel. Self-contained (only external dependency is the Leaflet CDN). |
| `data.json` | The list of historic points the map loads. Editable by you via the admin panel. |
| `README.md` | This file. |
| `images/` (you create) | Optional. Put photo files here and reference them as `images/photo.jpg`. |

> If `data.json` is missing (e.g. you open `index.html` directly with `file://`), the app
> falls back to a few sample points embedded inside `index.html`. On GitHub Pages it always
> loads `data.json`.

---

## Step-by-step: publish on GitHub Pages (free, ~10 minutes)

These steps assume you have a GitHub account. The instructions use the GitHub website; if
you know `git` on the command line you can do the same thing faster.

### 1. Create the repository

1. Sign in at **https://github.com**.
2. Click the **+** in the top-right → **New repository**.
3. Name it `tehran-historic-map` (or any name).
4. Set it to **Public** (Pages on free accounts requires public repos).
5. **Do not** add a README or `.gitignore` — leave it empty.
6. Click **Create repository**.

### 2. Upload the three files

1. In the new empty repo, click **uploading an existing file** (or **Add file → Upload files**).
2. Drag `index.html`, `data.json` and this `README.md` into the box.
3. Scroll down, type a commit message like *"Initial historic map"*, click **Commit changes**.

### 3. Turn on GitHub Pages

1. In the repo, click the **Settings** tab.
2. In the left sidebar, click **Pages**.
3. Under **Build and deployment → Source**, pick **Deploy from a branch**.
4. Under **Branch**, select **main** and the **/ (root)** folder, then click **Save**.
5. Wait ~1 minute. Refresh the Pages settings page — you will see a green banner with a URL
   like `https://YOUR-USERNAME.github.io/tehran-historic-map/`.
6. Open that URL. Your map loads with the sample points.

### 4. Lock down the admin password

The default admin password is `admin1968`. **Change it before relying on it** — anyone can
read the source.

1. Open `index.html` in a text editor (or click the file on GitHub → ✏️ pencil icon).
2. Find this line near the top of the `<script>`:
   ```js
   ADMIN_PASSWORD:  "admin1968",
   ```
3. Replace `"admin1968"` with a strong password of your own.
4. Save / commit the change.

> ⚠️ Important limitation: because this is a static site with no server, the password is
> stored in the page source. It stops casual visitors from adding points in their own
> browser, but a determined person can read it. For a genuinely secure admin you would need
> a backend (see *Going further* below). For a personal research project this is fine.

### 5. Add photos (optional)

For each point you can enter a **Photo URL**. Two free options:

- **Self-hosted in this repo**: create an `images/` folder in the repo
  (Add file → Upload files, the folder name is created automatically if you type
  `images/photo.jpg` as the filename). Then use `images/photo.jpg` as the photo URL.
  Best for your own scans.
- **External host**: any direct image URL (must be `https://`). Useful for archive images.

---

## Using the app

### As a visitor

1. Open the published URL on your phone or laptop.
2. Tap **🎯 Find my location** (top-left). The browser asks for location permission — allow it.
   A green dot marks your position with an accuracy circle.
3. Tap **📍 Nearby** (top-right) to open a side panel listing every historic point within
   the selected radius, sorted by distance. Tap any entry to fly to it and read its popup.
4. Tap any pin on the map to read its name, year, category, description, source and photo.
5. **🗺️ Layers** lets you switch between street map, satellite, terrain, and toggle a sepia
   "vintage" tint for a historic feel. You can also overlay a scanned 1968 map image
   (see below).

### As the admin

1. Tap **⚙️ Admin** (top-right), enter your password, click **Unlock**.
2. **Add a point**:
   - Click **➕ Add point**. The cursor becomes a crosshair.
   - Click the spot on the map where the historic place is. The lat/lng fill in
     automatically (you can also type them manually).
   - Fill in name, year, category, description, photo URL and source.
   - Click **💾 Save point**. The marker appears immediately.
   - Press **ESC** any time to cancel.
3. **Edit / delete**: each point appears in the **All points** list with **Edit** and
   **Del** buttons.
4. **Publish your changes** (so visitors see them, not just you):
   - Click **⬇️ Export JSON**. Your browser downloads a new `data.json`.
   - Go to your GitHub repo → click the existing `data.json` → ✏️ pencil icon →
     **Replace** / upload the new file (or delete it and upload the new one) →
     **Commit changes**.
   - Within a minute or two GitHub Pages refreshes and every visitor sees the updated map.
5. **Move between devices**: use **⬆️ Import JSON** to load a previously exported
   `data.json` and keep editing.

> Your edits are saved in your browser's `localStorage` until you export. Clearing your
> browser data loses them, so **export often**.

### Optional: overlay a scanned 1968 Tehran map

If you have a georeferenced scan (or can estimate the corners):

1. Host the image in the repo, e.g. `images/tehran-1968.jpg`.
2. Open **🗺️ Layers**, scroll to **Historic map image overlay**.
3. Enter the image URL and the **south, north, west, east** lat/lng of the image's four
   corners. (To get these, open [geojson.io](https://geojson.io) or Google Maps and right-
   click the four corners of the area your scan covers.)
4. Click **Apply overlay**. The scan renders on top of the modern map.

---

## Going further (later, when you outgrow this)

This static design is perfect for one city and a few hundred points. When you want
multi-admin, user-submitted points, or password security that isn't in the page source,
the natural next step is a tiny backend:

- **Supabase / PocketBase** (free tiers) for a real database + auth, with the same Leaflet
  frontend.
- Or keep it static but move the admin password to a serverless function
  (**Cloudflare Pages Functions** or **Netlify Functions**, both free) that writes to the
  repo via the GitHub API.

For now, the GitHub-Pages + `data.json` workflow handles everything you described.

---

## Data format

Each point in `data.json` is:

```json
{
  "id": "unique-string",
  "name": "Building name",
  "lat": 35.6892,
  "lng": 51.3890,
  "year": 1968,
  "category": "palace",
  "description": "What was here in 1968…",
  "photo": "images/photo.jpg",
  "source": "Book / archive / personal record",
  "sample": false
}
```

Categories: `palace`, `mosque`, `bazaar`, `museum`, `square`, `park`, `building`,
`monument`, `other` (each has its own emoji + colour).

---

## Credits

- Map library: [Leaflet.js](https://leafletjs.com) (BSD-2-Clause)
- Base tiles: [OpenStreetMap](https://www.openstreetmap.org/copyright) contributors,
  [Esri World Imagery](https://www.arcgis.com/home/item.html?id=10df2279f9684e4a9f6a7f08febac2a9),
  [OpenTopoMap](https://opentopomap.org) (CC-BY-SA)
- The sample points are well-known Tehran landmarks that existed in 1968. Coordinates are
  approximate and the entries are marked as samples — **replace them with your own verified
  research** before treating the map as authoritative.
