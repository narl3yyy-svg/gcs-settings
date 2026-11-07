# gcs-settings

Browser ground-station page for the RFD900 / ESP32 ground station. Live at
https://narl3yyy-svg.github.io/gcs-settings/ (Chrome or Edge on a PC, Chrome on Android).

## Changes

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
