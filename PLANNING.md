# ESP32-C3 CAN Bus PCB Project

## Project Overview
Design a compact PCB for automated manufacturing (JLCPCB compatible) featuring:
- ESP32-C3 microcontroller module
- TJA1050 CAN Bus transceiver
- RJ45 8-pin connector for CAN Bus communication
- Compact form factor suitable for industrial applications

## Architecture & Goals
- **Primary Goal**: Create a manufacturable PCB design for CAN Bus communication
- **Target Manufacturing**: JLCPCB or similar automated PCB assembly services
- **Form Factor**: Compact rectangular design (~80mm x 30mm based on image)
- **Power Supply**: 3.3V/5V compatible with onboard regulation

## Key Components
1. **ESP32-C3-MINI-1** or **ESP32-C3-WROOM-02** module
2. **TJA1050T CAN transceiver** (SOP-8 package for automated assembly)
3. **RJ45 8P8C connector** with integrated magnetics (optional)
4. **AMS1117-3.3 voltage regulator** for power management
5. **Supporting components**: resistors, capacitors, LEDs, crystal (if needed)

## PCB Specifications
- **Layers**: 2-layer PCB (cost-effective for JLCPCB)
- **Thickness**: 1.6mm standard
- **Copper Weight**: 1oz
- **Surface Finish**: HASL or ENIG
- **Solder Mask**: Green (standard)
- **Silkscreen**: White
- **Form Factor**: Vertical ESP32 orientation (as per reference image)

## Pin Assignments (ESP32-C3)
- **CAN_TX**: GPIO0 (TJA1050 pin 1 - TXD)
- **CAN_RX**: GPIO1 (TJA1050 pin 4 - RXD)
- **CAN_SILENT**: GPIO2 (TJA1050 pin 8 - S/Silent mode, optional)
- **STATUS_LED**: GPIO3 (Power/Status indication)
- **BOOT**: GPIO9 (Boot mode selection)

## Power Management
- **Input Voltage**: 5-12V DC via RJ45 or dedicated connector
- **Regulation**: AMS1117-3.3 for ESP32-C3 (3.3V)
- **CAN Transceiver**: 5V supply (from input or boost converter)
- **Protection**: Reverse polarity, overvoltage protection

## Manufacturing Constraints
- **Component Selection**: Use JLCPCB basic/extended parts library
- **Package Types**: Prefer SMD packages suitable for pick-and-place
- **Minimum Trace Width**: 0.15mm (6mil)
- **Minimum Via Size**: 0.2mm drill, 0.45mm pad
- **Component Spacing**: Follow JLCPCB assembly guidelines

## File Structure
```
/hardware/
  ├── schematic.kicad_sch          # KiCad schematic
  ├── pcb.kicad_pcb               # KiCad PCB layout
  ├── symbols/                    # Custom symbol library
  ├── footprints/                 # Custom footprint library
  └── gerbers/                    # Manufacturing files
/firmware/
  ├── src/                        # Source code
  ├── lib/                        # Libraries
  └── platformio.ini              # PlatformIO config
/manufacturing/
  ├── BOM.csv                     # Bill of Materials
  ├── CPL.csv                     # Component Placement List
  └── assembly_notes.md           # Manufacturing notes
```

## Design Rules
- Follow KiCad ERC/DRC with JLCPCB design rules
- Maintain 0.2mm minimum spacing between components
- Use standard footprints from KiCad libraries where possible
- Add test points for critical signals (CAN_H, CAN_L, 3.3V, GND)
- Include mounting holes (M3 or M2.5)

## Testing & Validation
- **Pre-production**: Order 5 PCBs for initial testing
- **Functional Tests**: CAN communication, power consumption, signal integrity
- **Environmental**: Temperature range, EMC compliance
- **Mechanical**: Connector retention, mounting stability