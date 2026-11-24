# gcs-settings

Browser ground-station page for the RFD900 / ESP32 ground station. Live at
https://narl3yyy-svg.github.io/gcs-settings/ (Chrome or Edge on a PC, Chrome on Android).

## Pages

- `index.html`: the full ground station (Status, Air, ELRS, Ground station with map, RFD900, About).
- `indexRFD.html`: only the RFD900 tools: Load/Save settings for both radios, RFD900
  temperatures, AT console and command reference. No third-party code, works offline. Live at
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
| File System Access (`showDirectoryPicker`, `showOpenFilePicker`, `showSaveFilePicker`) | Offline maps folder, `.pmtiles` files, saving the page | ✓ | |
| IndexedDB | Remembering which maps folder / map file was used (not the map data) | ✓ | |
| localStorage | Remembering the chosen map layer | ✓ | |
| Streams, TextEncoder, Blob, `fetch` | Serial data, saving tiles and files | ✓ | ✓ |

### Libraries built into `index.html` (map only)

The RFD900, Air, ELRS and Ground station code is written directly in the page (including the
MAVLink parser and the MD5 used for the ELRS binding ID). Only the map uses outside libraries,
and they are embedded in the file:

| Library | Version | License | Used for |
|---|---|---|---|
| [Leaflet](https://leafletjs.com) | 1.9.4 | BSD-2-Clause | Map, markers, trail, mission waypoints |
| [PMTiles](https://github.com/protomaps/PMTiles) | 4.5.0 | BSD-3-Clause | Reading offline `.pmtiles` street map files |
| [protomaps-leaflet](https://github.com/protomaps/protomaps-leaflet) | 5.1.0 | BSD-3-Clause | Drawing `.pmtiles` street maps |

Included inside protomaps-leaflet and PMTiles:

| Library | Version | License |
|---|---|---|
| @protomaps/basemaps | 5.7.2 | BSD-3-Clause |
| PMTiles (older copy) | 3.2.1 | BSD-3-Clause |
| @mapbox/vector-tile | 2.0.5 | BSD-3-Clause |
| pbf | 4.0.2 | BSD-3-Clause |
| @mapbox/point-geometry | 1.1.0 | ISC |
| potpack | 1.0.2 | ISC |
| quickselect | 2.0.0 | ISC |
| rbush | 3.0.1 | MIT |
| fflate | 0.8.3 | MIT |
| color2k | 2.0.4 | MIT |
| protocol-buffers-schema | 3.6.1 | MIT |
| resolve-protobuf-schema | 2.1.0 | MIT |

`indexRFD.html` contains no third-party code.

### Online services (optional)

| Service | Contacted when | Without internet |
|---|---|---|
| USGS The National Map (`basemap.nationalmap.gov`) | Satellite imagery (base map), and "Download area" | Use saved areas or a `.pmtiles` file |
| OpenAerialMap (`global.imagery.hotosm.org`) | Detail imagery from zoom 14 where available, and "Download area" with detail | Use saved areas |
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

- **USGS The National Map** (satellite base map, online and saved): public domain; free to use,
  including commercially and offline. USGS asks for a credit line: *"Map services and data
  available from U.S. Geological Survey, National Geospatial Program."* Imagery is credited "USDA,
  USGS The National Map". Coverage is the United States only; tiles go to zoom 16 (about 2 m per
  pixel) and are enlarged beyond that.
- **OpenAerialMap** (detail imagery from zoom 14, online and saved): © OpenAerialMap contributors,
  licensed under Creative Commons Attribution 4.0 International (CC BY 4.0,
  https://creativecommons.org/licenses/by/4.0/). Commercial use, offline copies and resale are
  allowed as long as the credit and license link are shown, which the map, saved areas and the
  About tab do. Imagery is shown unmodified. Coverage is wherever contributors have uploaded
  imagery (often drone or aerial surveys, a few cm to 1 m per pixel), so many areas have none.
  OpenAerialMap asks heavy users of its tile service to contact them; a commercial product with
  many users should do so.
- **OpenStreetMap** (`.pmtiles` street map files supplied by the user): © OpenStreetMap
  contributors, under the Open Database License (ODbL). Commercial use is allowed with this
  credit, which the map shows.
- **No longer used:** Esri (until 2026-11-23) and USGS NAIP / Topographic (until 2026-11-24).
  Areas saved from Esri with older versions must be deleted before selling or sharing the
  product: Esri's terms do not allow keeping their tiles offline or commercial use without a paid
  license. Areas saved from USGS NAIP or Topographic are public domain and can be kept.

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

### 2026-11-24: USGS + OpenAerialMap satellite map

- One satellite map: **USGS imagery** everywhere in the US (fast, about 2 m per pixel, enlarged
  past zoom 16) with **OpenAerialMap detail** drawn on top from zoom 14 wherever someone has mapped
  the area (often much sharper). Both are free for commercial use (public domain and CC BY 4.0).
- Layer list (top right): Satellite, "OpenAerialMap detail (zoom 14+)" (on), and
  "OpenAerialMap coverage (zoomed out)" (off; shows where detail exists).
- "Download area" always saves USGS zoom 8–16 and, if ticked, OpenAerialMap detail down to a
  chosen zoom (17–20). It checks each zoom-14 square first and only downloads deeper where
  imagery exists. Saved areas show both, offline.
- Removed: the slow NAIP layer, the Topographic layer and the old Esri code.

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
