# AC-Sensor Hardware Design

This directory contains the complete KiCad design files for the ESP32-C3 CAN Bus + 4x JST sensor board.

## Design Specifications

- **Board Size**: 80mm x 30mm (compact form factor)
- **Layers**: 2-layer PCB
- **Thickness**: 1.6mm standard
- **Manufacturing**: Optimized for JLCPCB automated assembly
- **JST Layout**: **2 connectors per side** for balanced cable management

## Key Components

| Component | Part Number | Package | Description |
|-----------|-------------|---------|-------------|
| U1 | ESP32-C3-MINI-1 | QFN-32 | Main microcontroller |
| U2 | TJA1050 | SO-8 | CAN transceiver |
| U3 | AMS1117-5.0 | SOT-223 | 5V regulator |
| J1 | RJ45_8P8C | RJ45 | CAN Bus connector |
| J2 | Conn_01x04 | Pin header | Programming interface |
| J3-J6 | JST_PH_3pin | JST 2.0mm | Sensor input connectors |

## JST Sensor Connectors Layout

**Balanced placement on both sides** for better cable management:

### LEFT SIDE (2 connectors)
| Connector | ESP32 Pin | Position | Description |
|-----------|-----------|----------|-------------|
| **J3** | GPIO0 (D0) | Left Top | Sensor 1: VCC, GND, D0 |
| **J4** | GPIO1 (D1) | Left Bottom | Sensor 2: VCC, GND, D1 |

### RIGHT SIDE (2 connectors)  
| Connector | ESP32 Pin | Position | Description |
|-----------|-----------|----------|-------------|
| **J5** | GPIO2 (D2) | Right Top | Sensor 3: VCC, GND, D2 |
| **J6** | GPIO3 (D3) | Right Bottom | Sensor 4: VCC, GND, D3 |

**JST Pinout (each connector):**
1. **VCC** - 5V power output for sensors
2. **GND** - Ground
3. **Signal** - Digital input to ESP32 (D0-D3)

## Board Layout

```
┌─────────────────────────────────────────────────────────────────────────────┐
│  [J3]              [POWER]    [ESP32-C3]   [TJA1050]        [RJ45]         │
│ SENSOR1              5V         CENTER       CAN             CAN            │
│  D0                                                                         │
│                                                                             │
│  [J4]                                                       [J5]           │
│ SENSOR2                                                    SENSOR3          │
│  D1                                                          D2             │
│                     [RST][BOOT]                            [J6]             │
│                                                           SENSOR4           │
│                                        [PROG]              D3              │
└─────────────────────────────────────────────────────────────────────────────┘
 LEFT SIDE                    CENTER                         RIGHT SIDE
```

## CAN Bus Interface

Based on the actual prototype implementation:

| Function | ESP32 Pin | Connection | Description |
|----------|-----------|------------|-------------|
| CAN TX | GPIO6 (D6) | Direct → TJA1050 TXD | CAN transmit |
| CAN RX | GPIO7 (D7) | TJA1050 RXD → 10kΩ → GPIO7 | CAN receive with protection |

**Protection**: 10kΩ resistor on CAN RX line for input protection

## RJ45 Pinout

| Pin | Signal | Description |
|-----|--------|-------------|
| 1 | +5VDC | Power input |
| 2 | GND | Ground |
| 3 | CAN-H | CAN Bus High |
| 4 | CAN-L | CAN Bus Low |
| 5-8 | Available | Future expansion |

## GPIO Assignments

| GPIO | Function | Connector | Position | Description |
|------|----------|-----------|----------|-------------|
| GPIO0 | D0 | J3 | Left Top | Sensor 1 digital input |
| GPIO1 | D1 | J4 | Left Bottom | Sensor 2 digital input |
| GPIO2 | D2 | J5 | Right Top | Sensor 3 digital input |
| GPIO3 | D3 | J6 | Right Bottom | Sensor 4 digital input |
| GPIO6 | CAN_TX | - | Internal | CAN transmit (D6) |
| GPIO7 | CAN_RX | - | Internal | CAN receive (D7) |
| GPIO9 | BOOT | SW2 | Bottom Left | Boot mode button |
| GPIO20 | UART_RX | J2 | Bottom Right | Programming RX |
| GPIO21 | UART_TX | J2 | Bottom Right | Programming TX |

## Design Features

### Balanced JST Layout
- **Left Side**: 2x JST connectors (J3, J4) for sensors 1-2
- **Right Side**: 2x JST connectors (J5, J6) for sensors 3-4  
- **Benefits**: Better cable management, balanced load, easier assembly

### Power Management
- 5V input via RJ45 connector (pin 1)
- AMS1117-5.0 linear regulator for stable 5V supply
- 5V provided to all JST sensor connectors (VCC pins)
- 3.3V internal regulation for ESP32-C3

### CAN Bus Interface
- TJA1050 transceiver for robust CAN communication
- 10kΩ input protection resistor on RX line
- RJ45 connector for easy field connections
- Standard CAN-H/CAN-L differential signaling

### Programming Interface
- 4-pin header (bottom right) for UART programming
- Reset and boot buttons (bottom left) for development
- Compatible with ESP32 programming tools

## Manufacturing Notes

### JLCPCB Compatibility
- All components selected from JLCPCB parts library
- Standard SMD packages for automated assembly
- Minimum trace width: 0.15mm (6 mil)
- Minimum via size: 0.2mm drill, 0.45mm pad

### Assembly Process
1. PCB fabrication (2-layer, 1.6mm, HASL finish)
2. SMD component placement and reflow
3. Through-hole connectors (JST, RJ45, programming header)
4. Programming and testing

### Cost Estimation
- Target cost: <$12 per board (qty 10)
- Includes PCB + assembly + components
- Based on JLCPCB pricing (2025)

## Sensor Compatibility

The JST connectors are designed for digital sensors with these characteristics:
- **Power**: 5V operation
- **Signal**: Digital output (3.3V/5V logic levels)
- **Examples**: Motion sensors, proximity sensors, digital temperature sensors, limit switches

## Cable Management Benefits

The **2-per-side JST layout** provides several advantages:
- **Balanced weight distribution** 
- **Shorter cable runs** to sensors on both sides
- **Reduced cable congestion** 
- **Easier installation** in tight spaces
- **Professional appearance** with organized cabling

## Version History

- **v1.0** (2025-06-02): Initial schematic design
- **v1.1** (2025-06-02): Updated to match actual prototype
  - Added 4x JST 2.0mm sensor connectors
  - Corrected CAN interface with 10kΩ protection resistor
  - Updated GPIO assignments (D0-D3, D6, D7)
  - Changed to 5V power supply for sensor compatibility
- **v1.2** (2025-06-02): **JST connector placement optimization**
  - **2x JST on LEFT side** (J3, J4 for D0, D1)
  - **2x JST on RIGHT side** (J5, J6 for D2, D3)
  - Balanced layout for better cable management
  - Matches actual prototype photo layout

## References

- [ESP32-C3 Datasheet](https://www.espressif.com/sites/default/files/documentation/esp32-c3_datasheet_en.pdf)
- [TJA1050 Datasheet](https://www.nxp.com/docs/en/data-sheet/TJA1050.pdf)
- [JST PH Series Connectors](https://www.jst-mfg.com/product/detail_e.php?series=199)
- [JLCPCB Design Rules](https://jlcpcb.com/capabilities/pcb-capabilities)
- [KiCad Documentation](https://docs.kicad.org/)

## Prototype Verification

The schematic and layout are based on analysis of the actual prototype boards, which clearly show:
- ESP32-C3 module in center
- **2x JST connectors on left side**
- **2x JST connectors on right side**  
- TJA1050 CAN transceiver
- RJ45 connector on right side  
- Reset/boot buttons
- Compact 80x30mm form factor in enclosure

This **balanced JST placement** is the optimal layout for professional sensor installations.
