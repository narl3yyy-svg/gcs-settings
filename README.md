# gcs-settings

Browser ground-station page for the RFD900 / ESP32 ground station. Live at
https://narl3yyy-svg.github.io/gcs-settings/ (Chrome or Edge on a PC, Chrome on Android).

## Pages

- `index.html`: the full ground station (Status, Air, ELRS, Ground station with map, RFD900, About).
  **Personal / non-commercial use:** its map uses Esri basemaps under Esri's terms (see
  [Map data](#map-data)), so this page is not sold.
- `indexRFD.html`: only the RFD900 tools: Load/Save settings for both radios, RFD900
  temperatures, AT console and command reference. No third-party code, no map content, works
  offline. **This is the commercial product.** Live at
  https://narl3yyy-svg.github.io/gcs-settings/indexRFD.html.

## Dependencies

Each page is a single self-contained HTML file. Nothing is installed or built, no server is
needed, and no scripts, styles or fonts are loaded from the internet.

### Browser

Chrome or Edge on a PC, or Chrome on Android (USB to the ESP32 only). Firefox and Safari lack
Web Serial and folder access, so the pages do not work there.

| Browser feature | Used for | `index.html` | `indexRFD.html` |
|---|---|---|---|
| Web Serial (`navigator.serial`) | USB connection on a PC (ESP32 or plain RFD900) | ✓ | ✓ |
| WebUSB (`navigator.usb`) | USB connection to the ESP32 on Android | ✓ | ✓ |
| File System Access (`showDirectoryPicker`, `showSaveFilePicker`) | Offline maps folder, saving the page | ✓ | |
| IndexedDB | Remembering which maps folder was used (not the map data) | ✓ | |
| localStorage | Remembering the chosen map, the map view and Private mode | ✓ | |
| Streams, TextEncoder, Blob, `fetch` | Serial data, saving tiles and files | ✓ | ✓ |

### Libraries built into `index.html` (map only)

The RFD900, Air, ELRS and Ground station code is written directly in the page (including the
MAVLink parser and the MD5 used for the ELRS binding ID). Only the map uses outside libraries,
and they are embedded in the file:

| Library | Version | License | Used for |
|---|---|---|---|
| [MapLibre GL JS](https://maplibre.org) | 5.24.0 | BSD-3-Clause | Map display: smooth zoom, rotation and tilt, markers, trail, mission route |

Included inside MapLibre GL JS:

| Library | Version | License |
|---|---|---|
| @mapbox/jsonlint-lines-primitives | 2.0.3 | MIT |
| @mapbox/point-geometry | 1.1.0 | ISC |
| @mapbox/tiny-sdf | 2.3.0 | BSD-2-Clause |
| @mapbox/unitbezier | 0.0.1 | BSD-2-Clause |
| @mapbox/vector-tile | 2.0.5 | BSD-3-Clause |
| @mapbox/whoots-js | 3.1.0 | ISC |
| @maplibre/geojson-vt | 6.3.3 | ISC |
| @maplibre/maplibre-gl-style-spec | 24.10.0 | ISC |
| @maplibre/mlt | 1.3.0 | MIT (dual MIT / Apache-2.0) |
| @maplibre/vt-pbf | 4.3.3 | MIT |
| earcut | 3.2.4 | ISC |
| gl-matrix | 3.4.4 | MIT |
| json-stringify-pretty-compact | 4.0.0 | MIT |
| kdbush | 4.1.0 | ISC |
| murmurhash-js | 1.0.0 | MIT |
| pbf | 4.0.2 | BSD-3-Clause |
| potpack | 2.1.0 | ISC |
| quickselect | 3.0.0 | ISC |
| tinyqueue | 3.0.0 | ISC |

MapLibre GL JS also contains code from mapbox-gl-js v1.13 (BSD-3-Clause), glfx.js (MIT) and
d3-color (BSD-3-Clause); these notices are part of its license text in the About tab.

`indexRFD.html` contains no third-party code.

### Online services (optional)

| Service | Contacted when | Without internet |
|---|---|---|
| Esri ArcGIS Online basemaps (`server.arcgisonline.com`) | Map in `index.html` (Satellite, Streets, Topographic), and "Download area" | Use saved areas (Private mode) |
| GitHub Pages | Opening the live site | Open a saved copy of the page |
| ardupilot.org | Only when a parameter name is clicked, to open its documentation | Not needed |

Neither page sends analytics or tracking data.

### Hardware and firmware

| Item | Needed for |
|---|---|
| RFD900 radios with SiK firmware | Radio settings, temperatures, AT console (standard `AT`/`RT` commands) |
| ESP32 ground station firmware | ELRS, bind, GPS, ground-station status and RC. Not needed for a plain RFD900. |
| FTDI or CP210x USB driver (Windows) | A plain RFD900 on a USB-serial cable |
| ArduPilot autopilot | Air, Mission and parameter features, and the armed check (MAVLink) |

## Licensing

### Third-party software

All embedded libraries are under permissive licenses (BSD-2-Clause, BSD-3-Clause, MIT, ISC)
that allow commercial use, modification and closed-source distribution, provided their copyright
notices and license texts are kept with the software. `index.html` includes them in its
**About** tab (component list, copyright lines and full license texts), which must stay in any
copy that is sold or distributed. The BSD-3-Clause licenses also forbid using their authors'
names to endorse or promote this product.

### Map data

- **Esri basemaps** (map in `index.html`, online and saved): World Imagery (Imagery © Esri, Maxar,
  Earthstar Geographics, and the GIS User Community), World Street Map and World Topographic Map
  (© Esri, HERE, Garmin, © OpenStreetMap contributors, and the GIS User Community). Used under
  Esri's terms of use. Those terms do not allow commercial use without an Esri license and limit
  keeping tiles offline, so the map feature is for **personal / non-commercial use only** and is
  **not part of the commercial product**. Users are responsible for complying with Esri's terms,
  including for areas saved with "Download area". The map shows Esri's credit lines.
- **Commercial product:** `indexRFD.html` contains no map content and no third-party code, so
  none of the above applies to it.
- **Earlier map sources** (USGS, NAIP, OpenAerialMap, `.pmtiles` street maps) are no longer used. Areas saved from them
  are public domain or CC BY 4.0 and can be kept.

### Privacy

- Neither page has accounts, analytics, tracking or advertising, or sends data about you, your
  aircraft or your radios anywhere. `indexRFD.html` makes no internet requests at all.
- `index.html` has **Private (offline only)** above the map, **on by default**: the map then
  loads nothing from the internet and shows only areas saved in your own folder ("Offline map").
- With Private off, online Esri maps and "Download area" fetch map squares from Esri. Like any
  online map, Esri then receives the computer's internet (IP) address and which squares are
  requested, which reveals the area being viewed. The page sends no cookies and not its own
  address (`<meta name="referrer" content="no-referrer">`, `credentials: 'omit'`).
- Saved areas, settings and missions stay on the PC; the browser remembers only the maps folder
  and a few page settings. Opening the page from GitHub Pages lets GitHub see that it was
  loaded; a copy saved with "Save this page to the PC" and opened from disk avoids that.

### Trademarks

RFD900 is a trademark of RFDesign, ESP32 of Espressif Systems; ExpressLRS, ArduPilot and MAVLink
are names of their respective projects. They are used only to describe compatibility. This
product is not affiliated with or endorsed by them.

### This project's own code

The code written for this project (`index.html` and `indexRFD.html`, apart from the embedded
third-party libraries above) is released under the **MIT License**, copyright (c) 2026
narl3yyy-svg. See [`LICENSE`](LICENSE).

In short: anyone may use, copy, change, share and sell it, as long as the copyright notice and
license text stay with it (credit to narl3yyy-svg). It comes with no warranty.

## Changes

### 2026-11-24: Private mode, offline map choice, street map files removed, ELRS power

- **Private (offline only)** above the map, on by default: no map requests at all; only saved
  areas are shown. Unticking it asks first and explains what Esri can see. "Download area" works
  only with Private off. No cookies or page address are sent with map requests.
- The base-map chooser's last entry is now **Offline map: <name>**: the saved area shown with
  "Show on map", read from the maps folder (choosing it switches the online maps off). "Show on
  map" selects it automatically.
- **Removed:** `.pmtiles` street map files, with PMTiles, @protomaps/basemaps and the built-in
  label font (about 290 KB smaller). The About tab, README and `LICENSE` list what remains.
- **ELRS output power:** "Match handset power" removed. The list shows the power levels the board
  reports (+10 dBm / 10 mW and +13 dBm / 20 mW on this board). Finer 1 dBm steps would need a
  firmware change on the ESP32.
- Privacy section added to the About tab and this README.

### 2026-11-24: Esri maps back (index.html for non-commercial use)

- The map in `index.html` uses **Esri** again: Satellite (sharp to about zoom 19, worldwide),
  Streets and Topographic, chosen at the top right of the map, in the MapLibre viewer (smooth
  zoom, rotation and tilt).
- "Download area" saves the chosen Esri map down to a chosen closest zoom (15–19), 8 tiles at a
  time; Stop and resume work as before.
- Because Esri's terms do not allow commercial use without a license, `index.html` and its map
  feature are for personal / non-commercial use. The product for sale is `indexRFD.html`, which
  has no map content. The About tab, this README and `LICENSE` say so.
- USGS imagery removed.

### 2026-11-24: Stop button fixed; high-detail NAIP option removed

- **Stop** in "Download area" now stops at once: requests still waiting on the map server are
  cancelled, and any request that gets no answer within 20 s is given up and retried later.
  Download again with the same name to resume.
- The short-lived "High detail (NAIP)" download option was removed (too slow). Saved areas use
  USGS imagery only.

### 2026-11-24: MapLibre map, USGS imagery only

- **Map viewer switched from Leaflet to MapLibre GL JS:** smooth zooming, rotation and tilt
  (right-drag or Ctrl-drag; the compass resets north), a scale bar, and fast drawing of
  `.pmtiles` street maps. Markers, aircraft trail, ground-to-aircraft line, mission waypoints
  (click to add, drag to move), saved offline areas and "Download area" all work as before.
- **Satellite map:** USGS National Map imagery only (public domain, United States, about 2 m per
  pixel, enlarged past zoom 16). The base map chooser (top right) switches between Satellite and
  an open offline street map.
- **Street map labels work offline:** Noto Sans label glyphs (Latin characters) are built in.
- **Removed:** OpenAerialMap, the NAIP layer, the Topographic layer, Leaflet and
  protomaps-leaflet. The About tab and this README list the new components and their licenses.
- `index.html` is now about 1.6 MB (MapLibre is larger than Leaflet).

### 2026-11-23: USGS maps, licenses, RFD-only page

- **Maps switched to the USGS National Map** (public domain, United States only) instead of Esri:
  Satellite (USGS imagery) and Topographic (USGS Topo). The USGS tile cache stops at zoom 16
  (about 2 m per pixel); closer zooms enlarge those tiles. "Download area" saves USGS tiles, with
  closest-zoom choices 13–16. Areas saved earlier from Esri still open.
- **About tab** listing every open-source component inside the page (Leaflet, PMTiles,
  protomaps-leaflet and the libraries they include) with its copyright and full license text, plus
  map-data credits (USGS, OpenStreetMap) and a trademark notice.
- **New `indexRFD.html`** with just the RFD900 tab and a Temperatures card (reads both radios'
  temperature). It shows the aircraft's armed state and mode from its heartbeat, and blocks radio
  commands while the aircraft is armed, as the full page does.
- RFD900 temperature readings are cleared when the device disconnects, like the radio settings.

### 2026-11-20: Unplugging is detected

- Pulling the USB cable now switches the page to **Not connected** within a couple of seconds,
  for both the ESP32 ground station and a plain RFD900. Before, the page could keep showing
  "Connected" because closing an unplugged port could hang.
- When the device disconnects (unplugged or Disconnect pressed), the RFD900 tab empties both the
  Ground radio and Aircraft radio columns, the same as after a page refresh, so old settings are
  never shown for a radio that is not connected. A radio command that was running ends with
  "the device was disconnected" instead of waiting to time out.

### 2026-11-07: Offline maps

A new **Offline maps** section on the Ground station tab (just above the NMEA console) lets the
map work with no internet. Map data is saved as files in a folder you choose on the PC or a USB
drive, never inside the browser.

- **Maps folder:** pick any folder. Each saved area is a subfolder holding the map tiles
  (`<zoom>/<x>/<y>.jpg`) and an `area.json` that describes it. Chrome remembers only which folder
  was used; after a restart press **Reconnect folder** to allow access again.
- **Save the area on screen:** pan and zoom the map to the area, give it a name, pick the map
  (Satellite, Streets or Topographic) and the closest zoom (15–19). The page shows the area in
  sq mi, the tile count and the approximate size before downloading, and asks for confirmation.
  Areas are limited to 200 sq mi each. A progress bar and a Stop button are shown; downloading
  again with the same name resumes and skips tiles already saved.
- **Saved areas:** lists every area in the folder. **Show on map** loads one (switch between them
  any time); **Hide** removes it. Saved areas also appear in the map's layer list.
- **Street map file (.pmtiles):** opens one vector street map file, which can cover a state or
  the whole US (about 15–25 GB for the US), read straight from disk. It appears as
  "Offline street map" in the layer list, with Light, Dark and Grey styles.
  *How to get a street map file* in the section explains how to cut one out of the free
  Protomaps / OpenStreetMap build with the `pmtiles` command-line tool.
- **Page works offline:** the map libraries (Leaflet 1.9.4, PMTiles 4.5.0, protomaps-leaflet
  5.1.0) are now built into `index.html` instead of loaded from the internet. Press
  **Save this page to the PC…** once while online, then open the saved file in Chrome or Edge
  when there is no internet. Radios, saved areas and street map files all work from it.
- Online maps (Esri Satellite, Streets, Topographic) work as before when there is internet.
  Saved imagery is © Esri and its providers; save only areas needed for your own flying.

### 2026-11-06: Direct RFD900 mode

- The Connect picker lists every COM port, not only ESP32 boards.
- A plain RFD900 on a USB-serial cable (no ESP32) is detected automatically. The page then runs the
  radio command sessions itself (+++, AT/RT commands, ATO) at the baud rate chosen next to Connect
  (57600 by default), so Load/Save settings, Read temperatures and the AT console work.
  ESP32-only features (ELRS, bind, GPS forwarding, board status) need the ESP32.
- Nothing changes when an ESP32 ground station is connected.

### 2026-10-19: RFD900 settings and temperature reads fixed

**Problem:** "Load settings" and "Read RFD900 temperatures" sometimes failed with
*Error: no reply from the ground station*, especially after the board had been plugged
in for a while. Unplugging and replugging the board was the only way to get a reading.

**Main cause:** after any small serial error (for example the browser falling behind on
incoming telemetry while the tab was busy or in the background), the page stopped reading
from the board for good. Every reply after that was lost until the board was replugged.

**Fixes:**

- The page keeps reading after a recoverable serial error instead of stopping. If reading
  really can't continue, a banner says to press Disconnect, then Connect.
- The serial receive buffer is 16 times larger (1 MB), so falling behind is less likely.
- Long replies from the board are kept. Reading both radios' settings returns more than
  1 KB, and the page used to throw away anything over 800 characters.
- A stray `#` byte in the telemetry, or one reply that came through cut short, no longer
  causes the next reply to be lost. If telemetry bytes get mixed into a reply, the page
  tries again with them removed.
- Nothing else is sent to the radios while they are in command mode (parameter reads,
  mission transfers, log downloads and aircraft commands wait until it's finished).
- Reads (Load settings, Read temperatures, read-only console commands) are tried a second
  time automatically if the first attempt fails. Writing settings, resetting to defaults
  and rebooting are never repeated.
- If the board reports an error during a radio command, it shows right away instead of
  after a timeout of up to 30 seconds.
- If sending to the board fails once, the page recovers instead of silently dropping
  every later command.
- Disconnect no longer hangs when the board has stopped accepting data, so reconnecting
  works without reloading the page.
- Also includes the v1.7 page changes: corrected Air tab layout and removed unused code.
