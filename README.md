# Arduino Morse Code Player

Type a message on your phone, send it over Bluetooth, and the Arduino plays it back in Morse code using a buzzer and an LED. An LCD screen shows the received text and the dots and dashes of the letter currently playing.

## Parts

- Arduino Uno
- HC-05 (or HC-06) Bluetooth module
- 16x2 LCD with I2C backpack (4 pins)
- Buzzer
- 10 female-to-male jumper wires
- USB cable
- Android phone with the **Serial Bluetooth Terminal** app (free on Google Play)

## Wiring (no breadboard)

The **female** end of each wire goes to the module pin and the **male** end goes to the Arduino.

### Bluetooth module

| HC-05 | Arduino |
|-------|---------|
| VCC   | 5V |
| GND   | GND (POWER header, next to 5V) |
| TXD   | Pin 2 |
| RXD   | Pin 3 |

### LCD with I2C

| LCD | Arduino |
|-----|---------|
| VCC | Pin 8 |
| GND | The other GND on the POWER header |
| SDA | A4 |
| SCL | A5 |

### Buzzer

| Buzzer | Arduino |
|--------|---------|
| + (red wire or long leg) | Pin 7 |
| - (black wire or short leg) | Top GND, next to pin 13 |

### Summary by Arduino pin

| Arduino | Connects to |
|---------|-------------|
| 5V | Bluetooth VCC |
| GND (POWER, 1st) | Bluetooth GND |
| GND (POWER, 2nd) | LCD GND |
| GND (next to pin 13) | Buzzer - |
| Pin 2 | Bluetooth TXD |
| Pin 3 | Bluetooth RXD |
| Pin 7 | Buzzer + |
| Pin 8 | LCD VCC |
| A4 | LCD SDA |
| A5 | LCD SCL |

## Wiring with a breadboard (alternative)

If you use a breadboard, share power through the rails:

1. Arduino 5V → **+** rail
2. Arduino GND → **-** rail
3. Bluetooth VCC and LCD VCC → **+** rail
4. Bluetooth GND, LCD GND, and buzzer **-** → **-** rail
5. TXD → Pin 2, RXD → Pin 3, SDA → A4, SCL → A5, buzzer **+** → Pin 7

Each module pin must sit in its own breadboard row, with its wire in the same row.

## Important notes

- Do not use pins 0 and 1: the USB connection needs them to upload code.
- Unplug the USB cable before moving wires.
- In the no-breadboard version, the LCD is powered from pin 8. If the screen looks dim, flickers, or the Arduino resets, use the breadboard version.
- The HC-05 RXD pin works at 3.3V. A direct connection works for testing, but a voltage divider (1k and 2k resistors) is the proper way.
- If your buzzer is **passive**, change `pasivo = false` to `pasivo = true` in the code.

## Phone app

Use **Serial Bluetooth Terminal** (by Kai Morich), a free Android app on Google Play. It sends the text you type to the HC-05 module.

1. Install it from Google Play.
2. Pair the HC-05 first in the phone's Bluetooth settings (PIN `1234` or `0000`).
3. Open the app, tap the menu (☰) → **Devices**, and choose the **Bluetooth classic** tab.
4. Tap your HC-05 (or HC-06) in the list.
5. Tap the plug icon at the top to connect. The module's LED should start blinking slowly.
6. Type your message in the box at the bottom and tap send.

The HC-05 and HC-06 use classic Bluetooth, so they work with Android but not with iPhone.

## Usage

1. Upload the code with the **Arduino Uno** board and the port that appears when you plug in the USB cable.
2. Pair the Bluetooth module with your phone (PIN `1234` or `0000`).
3. Open Serial Bluetooth Terminal and connect to the module.
4. Type a message, for example `SOS`, and send it.
5. The buzzer plays it in Morse, and the LCD shows the text on top and the dots and dashes below.

When a new message arrives, the previous one is cleared from the screen. Change the speed with `palabrasPorMinuto` (words per minute) in the code.

## Troubleshooting

| Problem | What to try |
|---------|-------------|
| The IDE can't find the board | Try another USB cable and port, and check the CH340 driver |
| The HC-05 doesn't show up on the phone | Check VCC and GND; the module's LED should blink fast |
| The LCD shows no text | Turn the contrast screw on the back, and try address `0x3F` instead of `0x27` |
| The buzzer only clicks | It's passive: use `pasivo = true` |
| Nothing arrives over Bluetooth | Check that TXD goes to pin 2 and RXD to pin 3 |
