# gcs-settings

Browser ground-station page for the RFD900 / ESP32 ground station. Live at
https://narl3yyy-svg.github.io/gcs-settings/ (Chrome or Edge on a PC, Chrome on Android).

## Changes

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
