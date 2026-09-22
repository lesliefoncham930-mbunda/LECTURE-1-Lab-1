# LECTURE-1-Lab-1
# ESP32-S3 RGB LED Lab — Lab Note

**Author:** Leslie Foncham
**Board:** ESP32-S3-DevKitC-1 v1.1 (onboard RGB LED, GPIO 38)
**IDE:** Arduino IDE 2.x, board package "esp32 by Espressif Systems"



## Learning Outcomes Covered

- Installed and configured the Arduino IDE for ESP32 boards.
- Verified serial I/O and uploaded sketches to an ESP32-S3.
- Used `neopixelWrite()` to control the onboard addressable RGB LED.
- Modified colour, timing, and sequence to observe how each change
  affects the LED's behaviour.

## Setup Summary

- Boards Manager URL added: `https://dl.espressif.com/dl/package_esp32_index.json`
- Board selected: **ESP32S3 Dev Module**
- Port used: `[FILL IN — e.g. COM5 or /dev/tty.usbmodemXXXX]`
- USB CDC On Boot: `[FILL IN — Enabled/Disabled, whichever worked for you]`
- Serial Monitor baud rate: 115200

## Sanity Check — Serial Monitor

Uploaded the minimal "Hello from ESP32-S3" sketch and confirmed tick
messages appeared correctly at 115200 baud.

**Screenshot:** `images/serial-monitor.png`
`[FILL IN — replace this file with your own Serial Monitor screenshot
showing the "Hello from ESP32-S3" message and/or the state messages
printed by the final sketch below]`

---

## Task A — Change the Colour

Modified the base blink sketch to cycle through green, blue, and white
by changing the RGB arguments passed to `neopixelWrite()`:

```cpp
// Green
neopixelWrite(RGB_BUILTIN, 0, RGB_BRIGHTNESS, 0);
delay(1000);
neopixelWrite(RGB_BUILTIN, 0, 0, 0);
delay(1000);

// Blue
neopixelWrite(RGB_BUILTIN, 0, 0, RGB_BRIGHTNESS);
delay(1000);
neopixelWrite(RGB_BUILTIN, 0, 0, 0);
delay(1000);

// White
neopixelWrite(RGB_BUILTIN, RGB_BRIGHTNESS, RGB_BRIGHTNESS, RGB_BRIGHTNESS);
delay(1000);
neopixelWrite(RGB_BUILTIN, 0, 0, 0);
delay(1000);
```

**What changed:** Only the three colour arguments inside
`neopixelWrite()`. The timing (`delay(1000)`) was left exactly as in
the original sketch.

**Expected effect:** The LED should blink the same on/off rhythm as
the original red version, just in a different colour each time —
green, then blue, then white, with a full second on and a full second
off.

**Observed effect:** `[FILL IN — did the LED show the correct colours?
Was white noticeably brighter or different in tone from the others?]`

---

## Task B — Change the Blink Rate

Changed both `delay(1000)` calls to `delay(250)`:

```cpp
void loop() {
  neopixelWrite(RGB_BUILTIN, RGB_BRIGHTNESS, 0, 0);
  delay(250);

  neopixelWrite(RGB_BUILTIN, 0, 0, 0);
  delay(250);
}
```

**What changed:** Only the delay values, from 1000 ms to 250 ms. The
colour logic is untouched.

**Expected effect:** The LED should blink four times faster than the
original — visibly flickering rather than the slow, deliberate
on/off pulse from the 1-second version.

**Observed effect:** `[FILL IN — how did the faster blink actually
look? Did it feel like a "flicker" as expected, or something else?]`

---

## Task C — RGB Cycle

Created a repeating Red → Green → Blue → Off sequence with a 500 ms
delay between each state:

```cpp
void loop() {
  neopixelWrite(RGB_BUILTIN, RGB_BRIGHTNESS, 0, 0);   // Red
  delay(500);

  neopixelWrite(RGB_BUILTIN, 0, RGB_BRIGHTNESS, 0);   // Green
  delay(500);

  neopixelWrite(RGB_BUILTIN, 0, 0, RGB_BRIGHTNESS);   // Blue
  delay(500);

  neopixelWrite(RGB_BUILTIN, 0, 0, 0);                // Off
  delay(500);
}
```

**What changed:** Instead of two states (on/off) with one colour, the
loop now steps through four distinct states in sequence, each with its
own `neopixelWrite()` call and its own 500 ms hold.

**Expected effect:** The LED should visibly cycle red → green → blue →
off, continuously, each colour held for half a second before moving to
the next.

**Observed effect:** `[FILL IN — did the cycle run smoothly? Was
500 ms comfortable to watch, or did it feel too fast/slow?]`

---

## Task D — Custom Pattern ("Signal Lamp")

This is the **final sketch**, `ESP32_RGB_Lab.ino`. It defines a
4-state pattern using three colours beyond off, each with its own
brightness and timing:

| State  | Red | Green | Blue | Hold time |
|--------|----:|------:|-----:|----------:|
| Amber  | 45  | 20    | 0    | 400 ms    |
| Purple | 25  | 0     | 35   | 500 ms    |
| Cyan   | 0   | 50    | 50   | 400 ms    |
| Off    | 0   | 0     | 0    | 600 ms    |

```cpp
void loop() {
  neopixelWrite(RGB_BUILTIN, 45, 20, 0);   // Amber
  Serial.println("State: AMBER (warning)");
  delay(400);

  neopixelWrite(RGB_BUILTIN, 25, 0, 35);   // Purple
  Serial.println("State: PURPLE (standby)");
  delay(500);

  neopixelWrite(RGB_BUILTIN, 0, 50, 50);   // Cyan
  Serial.println("State: CYAN (active)");
  delay(400);

  neopixelWrite(RGB_BUILTIN, 0, 0, 0);     // Off
  Serial.println("State: OFF (pause)");
  delay(600);
}
```

**Design choices:** Unlike Tasks A–C, each state here has both a
unique colour *and* a unique brightness/hold time, rather than reusing
the same brightness and delay for every colour. A `Serial.println()`
call was added for each state so the Serial Monitor output can be
matched directly against the physical LED colour when taking the
required screenshot.

**Observed effect:** `[FILL IN — describe how your "Signal Lamp"
pattern actually looked and behaved on the real board]`

---

## Troubleshooting Notes

`[FILL IN — leave this section blank if you had no issues, or note
anything from the troubleshooting guide you actually needed, e.g.
"had to hold BOOT and tap EN once to get the upload to start"]`

## Conclusion

`[FILL IN — 2-3 sentences: what these four tasks showed you about how
neopixelWrite() timing and colour arguments control the onboard RGB
LED, and anything that surprised you]`

---

## Repository Structure

```
README.md
ESP32_RGB_Lab.ino
images/serial-monitor.png
```
