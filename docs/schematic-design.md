# Schematic Design with Custom RJ45 Pinout

## RJ45 Connector Wiring

### Pin Assignment
```
RJ45 Pin 1: +5VDC    → Power Input (through protection)
RJ45 Pin 2: GND      → Ground Plane
RJ45 Pin 3: CAN-H    → TJA1050 Pin 7 (CANH)
RJ45 Pin 4: CAN-L    → TJA1050 Pin 6 (CANL)
RJ45 Pin 5: SDA      → ESP32-C3 GPIO4 (with pull-up)
RJ45 Pin 6: SCL      → ESP32-C3 GPIO5 (with pull-up)
RJ45 Pin 7: N/C      → Not Connected
RJ45 Pin 8: N/C      → Not Connected
```

## Power Supply Circuit

### Input Power (5V from RJ45)
```
RJ45 Pin 1 (+5V) → 
├── Polyfuse F1 (500mA protection)
├── TVS Diode D5 (transient protection)  
├── Input Filter C1 (470µF)
└── AMS1117-3.3 Input

AMS1117-3.3 Output (3.3V) →
├── Output Filter C2 (22µF)
├── Decoupling C3 (100nF)
└── ESP32-C3 VCC
```

### 5V Rail Distribution
```
5V Input →
├── TJA1050 VCC (Pin 3)
├── I2C Pull-up Resistors (via jumper)
└── Power LED D1 (Red)
```

## CAN Bus Circuit

### TJA1050T Connections
```
Pin 1 (TXD)  ← ESP32-C3 GPIO0
Pin 2 (GND)  → Ground
Pin 3 (VCC)  ← 5V Rail
Pin 4 (RXD)  → ESP32-C3 GPIO1
Pin 5 (VREF) → Not Connected
Pin 6 (CANL) → RJ45 Pin 4 + TVS Protection
Pin 7 (CANH) → RJ45 Pin 3 + TVS Protection  
Pin 8 (S)    ← ESP32-C3 GPIO2 (Silent Mode)
```

### CAN Bus Protection
```
CAN-H Line:
RJ45 Pin 3 → TVS Diode D6 → TJA1050 Pin 7
           ↓
         Ground (ESD protection)

CAN-L Line:  
RJ45 Pin 4 → TVS Diode D7 → TJA1050 Pin 6
           ↓
         Ground (ESD protection)
```

### CAN Termination (Optional)
```
Jumper JP1 enables/disables 120Ω termination:
CAN-H ←→ 120Ω Resistor R1 ←→ CAN-L
```

## I2C Interface Circuit

### ESP32-C3 I2C Connections
```
ESP32-C3 GPIO4 (SDA) →
├── 4.7kΩ Pull-up R6 (to 3.3V or 5V via jumper)
└── RJ45 Pin 5

ESP32-C3 GPIO5 (SCL) →  
├── 4.7kΩ Pull-up R7 (to 3.3V or 5V via jumper)
└── RJ45 Pin 6
```

### I2C Voltage Level Selection
```
Jumper JP2 selects I2C pull-up voltage:
Position 1: 3.3V (for 3.3V I2C devices)
Position 2: 5V (for 5V I2C devices)
```

## Status Indicators

### LED Circuit
```
Power LED (Red) D1:
5V → 330Ω R4 → LED D1 → GND

Status LED (Green) D2:
ESP32-C3 GPIO3 → 330Ω R5 → LED D2 → GND

CAN TX LED (Yellow) D3:
ESP32-C3 GPIO8 → 330Ω R8 → LED D3 → GND

CAN RX LED (Blue) D4:
ESP32-C3 GPIO10 → 330Ω R9 → LED D4 → GND
```

## Programming Interface

### UART Programming
```
ESP32-C3 GPIO21 (TX) → Programming Header Pin 3
ESP32-C3 GPIO20 (RX) ← Programming Header Pin 4
3.3V → Programming Header Pin 1
GND → Programming Header Pin 2
```

### Boot/Reset Control
```
Boot Button SW1:
ESP32-C3 GPIO9 ← SW1 ← 10kΩ R2 ← 3.3V

Reset Button SW2:  
ESP32-C3 EN ← SW2 ← 10kΩ R3 ← 3.3V
```

## PCB Layout Considerations

### Signal Routing Priority
1. **Power Distribution**: Wide traces for 5V and 3.3V rails
2. **CAN Differential Pair**: 120Ω impedance, length matched
3. **I2C Lines**: Keep traces short, add test points
4. **Crystal**: Close to ESP32 with ground guard rings

### Ground Plane Strategy
```
Solid ground plane on bottom layer
Via stitching for thermal and electrical connection
Separate analog/digital ground areas if needed
Ground guards around crystal oscillator
```

### Component Placement
```
Layout (Top View):
[RJ45] [ESP32-C3] [TJA1050] [AMS1117]
  |       |         |         |
[LEDs]  [Crystal] [Decoup]  [Power]
```

## Design Validation Checklist

### Electrical Verification
- [ ] 5V input protection circuit sized correctly
- [ ] 3.3V regulation stable under load
- [ ] CAN differential impedance = 120Ω ±10%
- [ ] I2C pull-up values appropriate for bus loading
- [ ] All GPIOs within ESP32-C3 electrical limits

### Signal Integrity
- [ ] CAN-H/CAN-L routed as differential pair
- [ ] I2C traces < 10cm for reliable operation
- [ ] Crystal traces < 5mm with ground guard
- [ ] Power supply ripple < 50mV p-p

### EMC Considerations
- [ ] TVS diodes on all external connections
- [ ] Ferrite beads on power lines if needed
- [ ] Ground plane coverage > 90%
- [ ] Connector shield connected to chassis ground

This design provides a complete CAN + I2C interface with proper power distribution and protection, perfectly suited for your custom RJ45 pinout requirements.