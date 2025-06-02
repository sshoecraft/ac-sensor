# Assembly and Manufacturing Notes
## ESP32-C3 CAN Bus PCB

### Pre-Assembly Checklist
- [ ] Verify all LCSC part numbers are in stock
- [ ] Check PCB thickness matches connector requirements (1.6mm)
- [ ] Confirm RJ45 connector footprint matches selected part
- [ ] Validate ESP32-C3 module orientation (antenna side)

### Component Orientation Critical Notes

#### U1 - ESP32-C3-MINI-1 Module
- **Pin 1 Position**: Bottom-left when viewed from top
- **Antenna Orientation**: Keep antenna area clear of copper pour
- **Soldering**: Use low-temperature profile (<250°C peak)

#### U2 - TJA1050T CAN Transceiver  
- **Pin 1 Position**: Top-left in SOP-8 package
- **Heat Sensitive**: Standard reflow profile acceptable
- **Critical Connections**: Verify CAN_H/CAN_L routing before assembly

#### U3 - AMS1117-3.3 Voltage Regulator
- **Tab Orientation**: Heat sink tab toward board center for thermal relief
- **Input/Output**: Verify correct input/output pin connections
- **Thermal Pad**: Ensure good solder joint to copper pour

### Special Assembly Instructions

#### Crystal Y1 (40MHz)
- **Placement**: Must be within 10mm of ESP32-C3 module
- **Load Capacitors**: C4 placement critical - keep traces short
- **Ground Guard**: Verify ground plane surrounds crystal area
- **Handling**: Use ESD precautions - crystal is sensitive

#### RJ45 Connector J1
- **Through-Hole Mounting**: May require selective wave soldering
- **Mechanical Stress**: Ensure proper support during insertion
- **Pin Assignment**: Verify CAN_H (pin 7) and CAN_L (pin 2) connections
- **Height Clearance**: Check enclosure compatibility

#### LEDs D1, D2, D3
- **Polarity**: Cathode toward ground (marked with line on package)
- **Current Limiting**: 330Ω resistors provide ~7mA drive current
- **Visibility**: Consider light pipe compatibility in enclosure

### Solder Paste and Reflow Profile

#### Paste Application
- **Stencil Thickness**: 0.12mm (5 mil)
- **Aperture Ratio**: 0.7-0.8 for small components
- **ESP32 Module**: May require stepped stencil for proper paste volume

#### Reflow Temperature Profile
```
Stage 1: Preheat    25°C → 150°C    60-90 seconds
Stage 2: Soak       150°C → 180°C   60-120 seconds  
Stage 3: Reflow     180°C → 245°C   30-60 seconds
Stage 4: Cool       245°C → 100°C   Natural cooling
```

### Quality Control Checkpoints

#### Visual Inspection (100% required)
- [ ] Component orientation correct (especially polarized parts)
- [ ] No solder bridges on fine-pitch components
- [ ] Adequate solder joint formation
- [ ] No missing or shifted components

#### Electrical Testing (Sample basis)
- [ ] Power-on test: 3.3V ±5% on test point
- [ ] Current consumption: <200mA @ 5V input
- [ ] CAN transceiver: Verify differential output
- [ ] GPIO functionality: LED control working

#### Functional Testing (First article)
- [ ] Firmware programming via UART successful
- [ ] CAN bus communication verified with analyzer
- [ ] WiFi functionality confirmed (if using ESP32-C3 WiFi)
- [ ] Button functionality verified

### Packaging and Handling

#### ESD Protection
- **Component Handling**: Use ESD workstation and wrist straps
- **Storage**: Anti-static bags for completed assemblies
- **Shipping**: ESD-safe packaging materials

#### Mechanical Protection
- **Connector Protection**: Protect RJ45 during shipping
- **PCB Support**: Avoid flexing during handling
- **Conformal Coating**: Consider for harsh environments

### Troubleshooting Common Issues

#### Power Supply Problems
- **Symptom**: No 3.3V output from regulator
- **Check**: Input voltage present, regulator orientation, output capacitor
- **Solution**: Verify C1, C2 placement and values

#### CAN Communication Failure
- **Symptom**: No CAN messages transmitted/received
- **Check**: TJA1050 power supply (5V), GPIO connections, termination
- **Solution**: Verify R1 (120Ω) placement, check differential signals

#### Programming Issues
- **Symptom**: Cannot program ESP32-C3
- **Check**: Boot button function, UART connections, power supply
- **Solution**: Verify SW1, R2 placement, check EN pin connection

### Documentation Requirements

#### Delivery Package
- [ ] Assembly drawings with component reference designators
- [ ] Bill of Materials with LCSC part numbers
- [ ] Component placement list (CPL) file
- [ ] Gerber files and drill files
- [ ] Assembly notes (this document)
- [ ] Test procedures and acceptance criteria

#### Traceability
- [ ] Lot numbers for critical components (ESP32, CAN transceiver)
- [ ] Assembly date and operator identification
- [ ] Test results and inspection records
- [ ] Rework history (if applicable)

### Cost Optimization Notes

#### Volume Considerations
- **<10 units**: Hand assembly may be cost-effective
- **10-50 units**: JLCPCB assembly recommended
- **>50 units**: Consider local assembly for faster turnaround

#### Component Alternatives
- **ESP32-C3-WROOM-02**: Alternative to MINI-1 if stock issues
- **SN65HVD230**: Alternative CAN transceiver (3.3V operation)
- **Generic RJ45**: Multiple suppliers available for connector

### Revision Control
- **Document Version**: 1.0
- **Last Updated**: June 2, 2025
- **Next Review**: After first production run
- **Change Control**: Update for any component substitutions