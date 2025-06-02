# AC-Sensor Hardware Design

This directory contains the complete KiCad design files for the ESP32-C3 CAN Bus + I2C sensor board.

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
| U3 | AMS1117-3.3 | SOT-223 | 3.3V regulator |
| J1 | RJ45_8P8C | RJ45 | Custom pinout connector |
| J2 | Conn_01x04 | Pin header | Programming interface |

## Custom RJ45 Pinout

| Pin | Signal | Description |
|-----|--------|--------------|
| 1 | +5VDC | Power input |
| 2 | GND | Ground |
| 3 | CAN-H | CAN Bus High |
| 4 | CAN-L | CAN Bus Low |
| 5 | SDA | I2C Data |
| 6 | SCL | I2C Clock |
| 7 | N/C | Not connected |
| 8 | N/C | Not connected |

## GPIO Assignments

| GPIO | Function | Description |
|------|----------|-------------|
| GPIO0 | CAN_TX | CAN transmit |
| GPIO1 | CAN_RX | CAN receive |
| GPIO2 | CAN_SILENT | CAN silent mode |
| GPIO3 | STATUS_LED | Status indicator |
| GPIO4 | I2C_SDA | I2C data line |
| GPIO5 | I2C_SCL | I2C clock line |
| GPIO8 | CAN_TX_LED | CAN TX activity |
| GPIO9 | BOOT | Boot mode button |
| GPIO10 | CAN_RX_LED | CAN RX activity |
| GPIO20 | UART_RX | Programming RX |
| GPIO21 | UART_TX | Programming TX |

## Design Features

### Power Management
- 5V input via RJ45 connector
- AMS1117-3.3 linear regulator
- 470µF input capacitor for filtering
- 100nF output capacitor for stability

### CAN Bus Interface
- TJA1050 transceiver for robust communication
- 120Ω termination resistor (R5)
- ESD protection on CAN lines
- Status LEDs for bus activity

### I2C Interface
- 4.7kΩ pull-up resistors (R3, R4)
- 3.3V or 5V selectable (via jumper)
- Supports standard and fast mode

### Programming Interface
- 4-pin header for UART programming
- Reset and boot buttons
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
3. Through-hole components (if any)
4. Programming and testing

### Cost Estimation
- Target cost: <$15 per board (qty 10)
- Includes PCB + assembly + components
- Based on JLCPCB pricing (2025)

## Next Steps

1. **PCB Layout**: Complete component placement and routing
2. **DRC Check**: Verify design rules compliance
3. **Manufacturing Files**: Generate Gerbers, drill files, BOM, CPL
4. **Prototype**: Order initial boards for testing
5. **Validation**: Test all interfaces and functionality

## Version History

- **v1.0** (2025-06-02): Initial schematic design
  - Complete electrical design
  - Component selection
  - Custom RJ45 pinout definition

## References

- [ESP32-C3 Datasheet](https://www.espressif.com/sites/default/files/documentation/esp32-c3_datasheet_en.pdf)
- [TJA1050 Datasheet](https://www.nxp.com/docs/en/data-sheet/TJA1050.pdf)
- [JLCPCB Design Rules](https://jlcpcb.com/capabilities/pcb-capabilities)
- [KiCad Documentation](https://docs.kicad.org/)
