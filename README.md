<div align="center">

# GCS Settings

**Browser ground station for RFD900 radios and the ESP32 ground station**

Single-file web pages · No install · Works offline · No tracking

[Open the full ground station](https://narl3yyy-svg.github.io/gcs-settings/) ·
[Open the RFD900 tool](https://narl3yyy-svg.github.io/gcs-settings/indexRFD.html) ·
[License](LICENSE)

</div>

---

## Contents

- [Pages](#pages)
- [Features](#features)
- [Getting started](#getting-started)
- [Offline use and privacy](#offline-use-and-privacy)
- [Dependencies](#dependencies)
- [Licensing](#licensing)
- [Changelog](#changelog)

## Pages

| Page | What it is | Use |
|---|---|---|
| [`index.html`](https://narl3yyy-svg.github.io/gcs-settings/) | Full ground station: Status, Air, ELRS, Ground station (map, missions, offline maps), RFD900 and About tabs | Personal / non-commercial (its map uses Esri basemaps) |
| [`indexRFD.html`](https://narl3yyy-svg.github.io/gcs-settings/indexRFD.html) | RFD900 tools only: radio settings, temperatures, AT console, command reference | **Commercial product.** No third-party code, no map content, no internet use |

## Features

| Area | Highlights |
|---|---|
| **RFD900 radios** | Load, compare and save settings on the ground and aircraft radios · copy required settings to the aircraft · reset to defaults · read temperatures · AT console with a full SiK command reference |
| **Connections** | ESP32 ground station (115200 baud) or a plain RFD900 on a USB-serial cable (direct mode, chosen baud) · recovers from serial hiccups · detects unplugging and clears stale readings |
| **Aircraft (ArduPilot)** | Live telemetry and armed state · flight modes and commands · parameters · missions (click to add waypoints, drag to move, Mission Planner `.waypoints` files) · flight logs |
| **ELRS and ground station** | Link status · output power · binding phrase and bind · RC rate · ground position for Follow mode · board name and reboot |
| **Map** | MapLibre viewer with smooth zoom, rotation and tilt · Esri Satellite, Streets and Topographic · ground, aircraft, home, trail and mission overlays |
| **Offline maps** | Save areas (up to 200 sq mi, zoom 15–19) as files in a folder you choose · stop and resume downloads · show a saved area as the **Offline map** with no internet |
| **Safety** | Radio commands, power changes and reboots that interrupt RC are blocked while the aircraft is armed and confirmed otherwise |

## Getting started

1. Open a page in **Chrome or Edge** on a PC (or Chrome on Android for the ESP32 over USB).
2. Plug in the ESP32 ground station or the RFD900's USB cable.
3. For a plain RFD900, pick its baud rate next to **Connect** (usually 57600).
4. Press **Connect** and choose the COM port.

> [!TIP]
> If the COM port does not appear, install the radio cable's USB driver (FTDI or CP210x) and
> close other programs that use the port, such as Mission Planner.

## Offline use and privacy

- **Private (offline only)** above the map is **on by default**: the map loads nothing from the
  internet and shows only areas saved in your own folder.
- To save an area, untick Private, frame the area on the map, then use **Offline maps → Download
  area**. Saved areas open instantly from disk afterwards.
- Use **Save this page to the PC** once, then open that copy from disk: everything (radios, map
  library, saved areas) works with no internet, and no website is contacted.
- Neither page has accounts, analytics, tracking or advertising. `indexRFD.html` makes no
  internet requests at all.

> [!NOTE]
> With Private off, online Esri maps and downloads fetch map squares from Esri. Like any online
> map, Esri then receives your internet (IP) address and which squares are requested. The page
> sends no cookies and not its own address.

## Dependencies

Each page is one self-contained HTML file: nothing to install or build, no server, and no
scripts, styles or fonts loaded from the internet.

<details>
<summary><b>Browser features</b></summary>

<br>

Chrome or Edge on a PC, or Chrome on Android (USB to the ESP32 only). Firefox and Safari lack
Web Serial and folder access.

| Feature | Used for | `index.html` | `indexRFD.html` |
|---|---|:---:|:---:|
| Web Serial | USB connection on a PC | ✓ | ✓ |
| WebUSB | USB connection to the ESP32 on Android | ✓ | ✓ |
| File System Access | Offline maps folder, saving the page | ✓ | |
| IndexedDB | Remembering the maps folder (not the map data) | ✓ | |
| localStorage | Remembering the chosen map, map view and Private mode | ✓ | |
| WebGL | Drawing the map | ✓ | |

</details>

<details>
<summary><b>Libraries built into <code>index.html</code></b> (map only)</summary>

<br>

All other code (radios, MAVLink, ELRS, missions) is written directly in the page.

| Library | Version | License |
|---|---|---|
| [MapLibre GL JS](https://maplibre.org) | 5.24.0 | BSD-3-Clause |
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
d3-color (BSD-3-Clause). `indexRFD.html` contains no third-party code.

</details>

<details>
<summary><b>Online services</b> (optional)</summary>

<br>

| Service | Contacted when | Without internet |
|---|---|---|
| Esri ArcGIS Online (`server.arcgisonline.com`) | Online map with Private off, and "Download area" | Saved areas (Private mode) |
| GitHub Pages | Opening the live site | A saved copy of the page |
| ardupilot.org | Only when a parameter name is clicked | Not needed |

</details>

<details>
<summary><b>Hardware and firmware</b></summary>

<br>

| Item | Needed for |
|---|---|
| RFD900 radios with SiK firmware | Radio settings, temperatures, AT console |
| ESP32 ground station firmware | ELRS, bind, GPS, ground-station status and RC (not needed for a plain RFD900) |
| FTDI or CP210x USB driver (Windows) | A plain RFD900 on a USB-serial cable |
| ArduPilot autopilot | Air, Mission and parameter features, and the armed check |

</details>

## Licensing

| Part | License | Notes |
|---|---|---|
| This project's code (`index.html`, `indexRFD.html`) | **MIT**, © 2026 narl3yyy-svg | Use, copy, change, share and sell freely; keep the copyright notice and license text. No warranty. See [`LICENSE`](LICENSE). |
| Embedded libraries (MapLibre GL JS and its parts) | BSD-2-Clause, BSD-3-Clause, MIT, ISC | Commercial use allowed. Their notices and full license texts are in the **About** tab and must stay with any copy. |
| Map content (Esri Satellite, Streets, Topographic) | Esri terms of use | **Personal / non-commercial use only**; not part of the commercial product. Users are responsible for following Esri's terms, including for saved areas. |

> [!IMPORTANT]
> The commercial product is **`indexRFD.html`**. It contains no map content and no third-party
> code, so the Esri terms above do not apply to it.

<details>
<summary><b>Map credits</b></summary>

<br>

- **Esri World Imagery:** Imagery © Esri, Maxar, Earthstar Geographics, and the GIS User Community.
- **Esri World Street Map and World Topographic Map:** © Esri, HERE, Garmin, © OpenStreetMap
  contributors, and the GIS User Community.
- **Earlier sources** (USGS, NAIP, OpenAerialMap, `.pmtiles` street maps) are no longer used.
  Areas saved from them are public domain or CC BY 4.0 and can be kept.

</details>

<details>
<summary><b>Trademarks</b></summary>

<br>

RFD900 is a trademark of RFDesign, ESP32 of Espressif Systems; ExpressLRS, ArduPilot and MAVLink
are names of their respective projects. They are used only to describe compatibility. This
product is not affiliated with or endorsed by them.

</details>

## Changelog

<details open>
<summary><b>2026-11-24</b> · Private mode, Offline map, Esri maps, layout</summary>

<br>

- **Private (offline only)** above the map, on by default: no map requests at all. Unticking it
  asks first. No cookies or page address are sent with map requests.
- The map chooser's **Offline map: &lt;name&gt;** shows the saved area from disk; **Show on map**
  selects it automatically.
- **Esri** Satellite (to about zoom 19), Streets and Topographic are back; downloads save the chosen
  map to zoom 15–19, 8 tiles at a time.
- **Stop** in "Download area" stops at once; requests time out after 20 s; resume by downloading
  again with the same name.
- **ELRS output power:** "Match handset power" removed; the list shows the levels the board reports
  (+10 dBm and +13 dBm). Finer 1 dBm steps would need an ESP32 firmware change.
- Ground station tab: the RC rate, ground position and board tiles now sit below Mission.
- Removed: `.pmtiles` street maps (with PMTiles, @protomaps/basemaps and the label font), USGS,
  NAIP and OpenAerialMap imagery.
- Map viewer switched from Leaflet to **MapLibre GL JS** (smooth zoom, rotation, tilt).

</details>

<details>
<summary><b>2026-11-23</b> · Licenses and the RFD900 page</summary>

<br>

- **About** tab with every embedded component's copyright and full license text, map credits and
  a trademark notice.
- New **`indexRFD.html`**: the RFD900 tools plus a Temperatures card, with the aircraft's armed
  state shown and radio commands blocked while armed.
- Temperature readings clear when the device disconnects.

</details>

<details>
<summary><b>2026-11-20</b> · Unplug detection</summary>

<br>

- Pulling the USB cable switches the page to **Not connected** within a couple of seconds (closing
  an unplugged port used to hang).
- Disconnecting empties both radio columns, so stale settings are never shown; a running radio
  command ends at once.

</details>

<details>
<summary><b>2026-11-07</b> · Offline maps</summary>

<br>

- **Offline maps** section: choose a maps folder, save the area on screen (up to 200 sq mi) with a
  size estimate, progress and resume, and show saved areas with no internet.
- The map library is built into the page, and **Save this page to the PC** makes a copy that runs
  fully offline.

</details>

<details>
<summary><b>2026-11-06</b> · Direct RFD900 mode</summary>

<br>

- The Connect picker lists every COM port.
- A plain RFD900 is detected automatically; the page runs the radio command sessions itself
  (`+++`, AT/RT commands, `ATO`) at the chosen baud rate. ESP32-only features need the ESP32.

</details>

<details>
<summary><b>2026-10-19</b> · Reliable RFD900 reads</summary>

<br>

- Fixed "no reply from the ground station" after a serial hiccup: the page now keeps reading
  instead of stopping until the board was replugged.
- 16× larger receive buffer; long replies kept; stray bytes no longer spoil the next reply.
- Nothing else is sent while the radios are in command mode; read-only commands retry once;
  board errors show at once.
- Recovers from a failed send, and Disconnect no longer hangs.

</details>
