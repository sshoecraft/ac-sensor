# AC-Sensor Hardware Design

This directory contains the complete KiCad design files for the ESP32-C3 CAN Bus + 4x JST sensor board.

## Files Structure

```
hardware/
├── ac-sensor.kicad_pro      # KiCad project file
├── schematic.kicad_sch       # Complete schematic design
├── pcb.kicad_pcb            # PCB layout (placeholder)
├── symbols/                  # Custom symbol libraries
├── footprints/              # Custom footprint libraries
└── gerbers/                 # Manufacturing files (generated)
```

## Design Specifications

- **Board Size**: 80mm x 30mm (compact form factor)
- **Layers**: 2-layer PCB
- **Thickness**: 1.6mm standard
- **Manufacturing**: Optimized for JLCPCB automated assembly
- **Components**: All SMD, 0805 packages where possible

## Key Components

| Component | Part Number | Package | Description |
|-----------|-------------|---------|-------------|
| U1 | ESP32-C3-MINI-1 | QFN-32 | Main microcontroller |
| U2 | TJA1050 | SO-8 | CAN transceiver |
| U3 | AMS1117-5.0 | SOT-223 | 5V regulator |
| J1 | RJ45_8P8C | RJ45 | CAN Bus connector |
| J2 | Conn_01x04 | Pin header | Programming interface |
| J3-J6 | JST_PH_3pin | JST 2.0mm | Sensor input connectors |

## JST Sensor Connectors

**4x JST 2.0mm PH Series 3-pin connectors** for external sensor inputs:

| Connector | ESP32 Pin | Signal | Description |
|-----------|-----------|--------|-------------|
| J3 | GPIO0 (D0) | VCC, GND, D0 | Sensor 1 input |
| J4 | GPIO1 (D1) | VCC, GND, D1 | Sensor 2 input |
| J5 | GPIO2 (D2) | VCC, GND, D2 | Sensor 3 input |
| J6 | GPIO3 (D3) | VCC, GND, D3 | Sensor 4 input |

**JST Pinout (per connector):**
1. **VCC** - 5V power output for sensors
2. **GND** - Ground
3. **Signal** - Digital input to ESP32 (D0-D3)

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

| GPIO | Function | Description |
|------|----------|-------------|
| GPIO0 | D0 | Sensor 1 digital input |
| GPIO1 | D1 | Sensor 2 digital input |
| GPIO2 | D2 | Sensor 3 digital input |
| GPIO3 | D3 | Sensor 4 digital input |
| GPIO6 | CAN_TX | CAN transmit (D6) |
| GPIO7 | CAN_RX | CAN receive (D7) |
| GPIO9 | BOOT | Boot mode button |
| GPIO20 | UART_RX | Programming RX |
| GPIO21 | UART_TX | Programming TX |

## Design Features

### Power Management
- 5V input via RJ45 connector (pin 1)
- AMS1117-5.0 linear regulator for stable 5V supply
- 5V provided to all JST sensor connectors (VCC pins)
- 3.3V internal regulation for ESP32-C3

### Sensor Interface
- 4x JST 2.0mm PH series connectors
- Each provides: 5V power, Ground, Digital signal
- Direct connection to ESP32 GPIO pins (D0-D3)
- Compatible with various digital sensors

### CAN Bus Interface
- TJA1050 transceiver for robust CAN communication
- 10kΩ input protection resistor on RX line
- RJ45 connector for easy field connections
- Standard CAN-H/CAN-L differential signaling

### Programming Interface
- 4-pin header for UART programming
- Reset and boot buttons for development
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

## Next Steps

1. **PCB Layout**: Complete component placement and routing
2. **DRC Check**: Verify design rules compliance  
3. **Manufacturing Files**: Generate Gerbers, drill files, BOM, CPL
4. **Prototype**: Order initial boards for testing
5. **Validation**: Test all interfaces and sensor compatibility

## Version History

- **v1.0** (2025-06-02): Initial schematic design
- **v1.1** (2025-06-02): Updated to match actual prototype
  - Added 4x JST 2.0mm sensor connectors
  - Corrected CAN interface with 10kΩ protection resistor
  - Updated GPIO assignments (D0-D3, D6, D7)
  - Changed to 5V power supply for sensor compatibility

## References

- [ESP32-C3 Datasheet](https://www.espressif.com/sites/default/files/documentation/esp32-c3_datasheet_en.pdf)
- [TJA1050 Datasheet](https://www.nxp.com/docs/en/data-sheet/TJA1050.pdf)
- [JST PH Series Connectors](https://www.jst-mfg.com/product/detail_e.php?series=199)
- [JLCPCB Design Rules](https://jlcpcb.com/capabilities/pcb-capabilities)
- [KiCad Documentation](https://docs.kicad.org/)

## Actual Implementation Photo

The schematic is based on analysis of the actual prototype board, which shows:
- ESP32-C3 module in center
- 4x JST connectors on left side
- TJA1050 CAN transceiver
- RJ45 connector on right side  
- Reset/boot buttons
- Compact form factor in blue enclosure
