# Smart LED Lamp

A Bluetooth-controlled smart lamp built around an ESP32-S3 and a nine-pixel WS2812B array. The project combines the power supply, controller, addressable lighting, and expansion connections on a custom KiCad PCB.

- **Forge project:** [Smart LED Lamp](https://forge.hackclub.com/projects/4251)
- **Controller:** ESP32-S3-WROOM-1
- **Lighting:** Nine WS2812B addressable RGB LEDs

## Hardware design

### Power input and regulation

- USB-C receptacle for 5 V input, with 5.1 kΩ resistors on the CC pins.
- 1 A fuse on the incoming supply.
- AP7361C-33 regulator for the 3.3 V controller rail.
- Regulator input/output capacitors, a power indicator LED, and supply test points.
- 10 µF and 100 nF decoupling on the ESP32-S3 3.3 V rail.

### ESP32-S3 controller

The ESP32-S3-WROOM-1 is the central controller. The schematic includes an enable/reset circuit with a pull-up, capacitor, and pushbutton, plus a boot button. USB data lines connect through 22 Ω series resistors. A four-pin connector exposes 3.3 V, ground, SDA, and SCL; the I²C lines have 4.7 kΩ pull-ups.

### Addressable LED array

Nine WS2812B LEDs are connected in a serial data chain. The ESP32 data output passes through a 74AHCT1G125 level shifter and a 330 Ω series resistor before the first LED. Each LED has a 100 nF supply capacitor, and a 470 µF capacitor provides bulk capacitance on the 5 V LED supply.

## PCB layout and footprints

The board has a rectangular outline, four corner mounting holes, and silkscreen branding with the project name and lamp icon. The ESP32-S3 sits near the top edge with its antenna area kept clear. USB-C and the power components are on the left; the boot/reset buttons are on the right; the regulator and support components sit below the input. The nine LEDs form a 3 × 3 array in the lower half, with their capacitors and the bulk capacitor nearby.

The layout uses front- and back-layer routing, vias, and copper zones to connect the power, controller, buttons, and LED data chain. Test points and connectors are placed around the controller section.

![KiCad PCB layout](https://cdn.hackclub.com/01a17f7a-5897-7636-98cb-0cfcd79b9cf6/clipboard.png)

Footprint assignments include 0805 resistors and most capacitors, a 6.3 × 7.7 mm electrolytic footprint for the 470 µF capacitor, 5 × 5 mm PLCC-4 footprints for the WS2812B LEDs, an 0805 indicator LED, a 1206 fuse, a USB-C receptacle, 2.54 mm connectors, tactile switches, and test-point pads.

## Rendering and visual review

A KiCad 3D render was used to review the assembled board view, component placement, LED spacing, board outline, mounting-hole positions, connector and switch access, and silkscreen readability. The render is a visual layout check; it does not demonstrate electrical operation.

## DRC and verification

The first DRC review reported zero errors and 12 warnings. The visible warnings included very short track ends on `Net-(D3-DOUT)` (0.0141 mm) and `VBUS` (0.0707 mm), silkscreen clipping for the `5V` and `BOOT` labels and the `R2` and `U3` reference fields, and a silkscreen overlap involving `D5`. The Unconnected Items tab showed zero items. These warnings were addressed during cleanup.

The final DRC report shows:

- 0 violations
- 0 unconnected items
- 0 schematic parity differences
- 0 errors
- 0 warnings

The report also lists 6 ignored tests and 8 exclusions separately.

## Build log

The work was recorded in stages:

1. [Schematic design](https://forge.hackclub.com/projects/4251/devlogs/60740) — USB-C power input, 3.3 V regulation, ESP32-S3, programming and I²C connections, and the LED data chain.
2. [Footprint assignment](https://forge.hackclub.com/projects/4251/devlogs/60748) — package selection for the passives, LEDs, connectors, switches, fuse, and test points.
3. [PCB design and layout](https://forge.hackclub.com/projects/4251/devlogs/60749) — board outline, mounting holes, component placement, routing, copper zones, and silkscreen.
4. [3D rendering and visual verification](https://forge.hackclub.com/projects/4251/devlogs/60750) — rendered assembly review.
5. [DRC warning cleanup](https://forge.hackclub.com/projects/4251/devlogs/60752) — review and cleanup of the initial warning report.

