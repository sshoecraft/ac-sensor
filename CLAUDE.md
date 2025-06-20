# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository Overview

This is a hardware design project for an ESP32-C3 based CAN Bus sensor board with multiple JST sensor connectors. The project focuses on creating a manufacturable PCB optimized for JLCPCB assembly services.

## Project Architecture

**ac-sensor/** - ESP32-C3 CAN Bus + 4x JST Sensor PCB
- Compact 80mm x 30mm form factor with balanced JST connector layout
- KiCad-based hardware design with complete schematic and manufacturing documentation
- Optimized for automated manufacturing at JLCPCB with cost target <$12/board
- Custom RJ45 pinout for CAN Bus communication and 5V power distribution

### Key Project Components

**hardware/** - KiCad design files and PCB layout
- Main schematic (schematic.kicad_sch) with ESP32-C3, TJA1050 CAN transceiver
- Custom symbols/ and footprints/ libraries for specialized components
- PCB layout (pcb.kicad_pcb) with balanced JST connector placement

**manufacturing/** - Production-ready files and documentation
- BOM.csv with LCSC part numbers for JLCPCB assembly
- CPL.csv component placement list for automated assembly
- assembly-notes.md with critical manufacturing instructions

**docs/** - Technical documentation and design specifications
- schematic-design.md with complete circuit analysis and GPIO assignments
- pinout-diagram.md and firmware-analysis.md for implementation details

## Hardware Specifications

### Board Architecture
- **Microcontroller**: ESP32-C3-MINI-1 (WiFi/BLE capable)
- **CAN Interface**: TJA1050 transceiver with RJ45 connector
- **Power Management**: AMS1117-5.0 regulator providing 5V for sensors
- **Sensor Interfaces**: 4x JST 2.0mm PH connectors (balanced 2-per-side layout)
- **Programming**: 4-pin UART header with reset/boot buttons

### Key Design Features
- **Balanced JST Layout**: 2 connectors on left side (J3,J4), 2 on right side (J5,J6)
- **CAN Bus Protection**: 10kΩ input protection resistor on RX line
- **GPIO Assignments**: D0-D3 (GPIO0-3) for sensors, D6/D7 (GPIO6/7) for CAN
- **Manufacturing Optimized**: All SMD components from JLCPCB parts library

## Development Commands

### KiCad Design Workflow
```bash
# Open schematic for editing
kicad hardware/schematic.kicad_sch

# Open PCB layout
kicad hardware/pcb.kicad_pcb

# Generate manufacturing files (Gerbers, drill files)
# Use KiCad Plot menu: File → Plot → Generate Drill Files
```

### Manufacturing File Generation
```bash
# Validate BOM against JLCPCB library
# Check manufacturing/BOM.csv for part availability

# Generate assembly files for JLCPCB
# Export CPL file from KiCad: File → Fabrication Outputs → Component Placement
```

## GPIO Pin Assignments

### Sensor Interface (Primary Function)
- **GPIO0 (D0)**: J3 Left Top - Sensor 1 digital input
- **GPIO1 (D1)**: J4 Left Bottom - Sensor 2 digital input  
- **GPIO2 (D2)**: J5 Right Top - Sensor 3 digital input
- **GPIO3 (D3)**: J6 Right Bottom - Sensor 4 digital input

### CAN Bus Interface
- **GPIO6 (D6)**: CAN TX → TJA1050 TXD
- **GPIO7 (D7)**: CAN RX ← TJA1050 RXD (with 10kΩ protection)

### Programming Interface
- **GPIO20**: UART RX (Programming header pin 4)
- **GPIO21**: UART TX (Programming header pin 3)
- **GPIO9**: Boot button (programming mode)

## JST Connector Layout

### Physical Placement (Balanced Design)
```
LEFT SIDE          CENTER           RIGHT SIDE
[J3] D0         [ESP32-C3]           [J5] D2
[J4] D1         [TJA1050]            [J6] D3
              [RJ45 CAN]
```

### JST Pinout (Each Connector)
1. **VCC** - 5V power output for sensors
2. **GND** - Ground reference
3. **Signal** - Digital input (GPIO0-3)

## Manufacturing Specifications

### JLCPCB Assembly Requirements
- **PCB**: 2-layer, 1.6mm thickness, HASL finish
- **Components**: All parts from JLCPCB basic/extended library
- **Assembly**: SMD components with automated pick-and-place
- **Cost Target**: <$12 per board (quantity 10)

### Design Rules
- **Minimum Trace**: 0.15mm (6 mil)
- **Minimum Via**: 0.2mm drill, 0.45mm pad
- **CAN Impedance**: 120Ω differential pair
- **Component Spacing**: 0.2mm minimum for automated assembly

## Project Status Tracking

The project uses TASK.md for detailed task tracking and completion status:
- **Schematic Design**: ✅ Complete with verified GPIO assignments
- **Component Selection**: ✅ Complete with JLCPCB part numbers
- **PCB Layout**: 🔄 In progress
- **Manufacturing Files**: ⏳ Pending PCB completion

## RJ45 Custom Pinout

| Pin | Signal | Description |
|-----|--------|-------------|
| 1 | +5VDC | Power input (fused, protected) |
| 2 | GND | Ground reference |
| 3 | CAN-H | CAN Bus High (with TVS protection) |
| 4 | CAN-L | CAN Bus Low (with TVS protection) |
| 5-8 | N/C | Available for future expansion |

## Design Validation

### Critical Checkpoints
1. **Power Supply**: 5V input → AMS1117-5.0 → clean 5V for sensors + 3.3V for ESP32
2. **CAN Interface**: Proper differential signaling with TJA1050 at 5V supply
3. **JST Layout**: Balanced connector placement matching prototype verification
4. **GPIO Protection**: 10kΩ resistor on CAN RX for input protection
5. **Manufacturing**: All components verified in JLCPCB parts library

### Testing Requirements
- Power-on test: Verify 5V regulation and current consumption <200mA
- CAN functionality: Message transmission/reception with bus analyzer
- Sensor interface: Digital input testing on all 4 JST connectors
- Programming: UART bootloader functionality via 4-pin header

## File Organization

```
ac-sensor/
├── hardware/          # KiCad design files
│   ├── schematic.kicad_sch
│   ├── pcb.kicad_pcb
│   ├── symbols/       # Custom component library
│   └── footprints/    # Custom footprint library
├── manufacturing/     # Production files
│   ├── BOM.csv       # Bill of materials with LCSC parts
│   ├── CPL.csv       # Component placement list
│   └── assembly-notes.md
├── docs/             # Technical documentation
│   ├── schematic-design.md    # Complete circuit analysis
│   ├── pinout-diagram.md      # GPIO and connector details
│   └── firmware-analysis.md   # Software compatibility
├── TASK.md           # Development task tracking
└── PLANNING.md       # Project architecture and goals
```

## Firmware Compatibility

The hardware design is compatible with ESP-IDF or Arduino framework with these pin definitions:

```c
// CAN Bus interface
#define CAN_TX GPIO_NUM_6     // D6 → TJA1050 TXD  
#define CAN_RX GPIO_NUM_7     // D7 ← TJA1050 RXD (protected)

// Sensor ports (4x JST connectors)
#define SENSOR_PORT_0 GPIO_NUM_0  // J3 Left Top
#define SENSOR_PORT_1 GPIO_NUM_1  // J4 Left Bottom  
#define SENSOR_PORT_2 GPIO_NUM_2  // J5 Right Top
#define SENSOR_PORT_3 GPIO_NUM_3  // J6 Right Bottom

// Programming interface
#define UART_RX GPIO_NUM_20
#define UART_TX GPIO_NUM_21
#define BOOT_BTN GPIO_NUM_9
```

This hardware platform provides a robust foundation for CAN Bus sensor networks with balanced mechanical design and manufacturing optimization for cost-effective production.