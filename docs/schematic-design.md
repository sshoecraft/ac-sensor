# Schematic Design with Custom RJ45 Pinout (Updated from Firmware)

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
├── Power LED D1 (Red)
└── Sensor Port Headers (optional 5V supply)
```

## CAN Bus Circuit (Updated Pin Assignment)

### TJA1050T Connections
```
Pin 1 (TXD)  ← ESP32-C3 GPIO6 (Updated from GPIO0)
Pin 2 (GND)  → Ground
Pin 3 (VCC)  ← 5V Rail
Pin 4 (RXD)  → ESP32-C3 GPIO7 (Updated from GPIO1)
Pin 5 (VREF) → Not Connected
Pin 6 (CANL) → RJ45 Pin 4 + TVS Protection
Pin 7 (CANH) → RJ45 Pin 3 + TVS Protection  
Pin 8 (S)    ← ESP32-C3 GPIO21 (Silent Mode)
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

## SPI Interface Circuit (New Addition)

### ESP32-C3 SPI Connections
```
ESP32-C3 GPIO8 (SPI_CLK) → Header J3 Pin 3
ESP32-C3 GPIO9 (SPI_MISO) → Header J3 Pin 4
ESP32-C3 GPIO10 (SPI_MOSI) → Header J3 Pin 5
3.3V → Header J3 Pin 1
GND → Header J3 Pin 2
GPIO11 (CS) → Header J3 Pin 6 (configurable)
```

## Generic Sensor Ports (Multi-function)

### GPIO0-3 Sensor Port Configuration
```
Each port (GPIO0, GPIO1, GPIO2, GPIO3):
├── 10kΩ Pull-up Resistor (via jumper JP3-JP6)
├── ESD Protection Diode
├── 3-pin Header (3.3V, GPIO, GND)
└── Multi-function capability:
    ├── Digital Input (Flow sensors, state monitoring)
    ├── Digital Output (Relay control)
    ├── OneWire Interface (DS18B20 temperature)
    └── Interrupt Input (Pulse counting)
```

### Sensor Port Headers (J4-J7)
```
Pin 1: 3.3V (or 5V via jumper)
Pin 2: GPIO Signal
Pin 3: GND
```

## Status Indicators (Updated)

### LED Circuit
```
Power LED (Red) D1:
5V → 330Ω R4 → LED D1 → GND

Status LED (Green) D2:
ESP32-C3 GPIO18 → 330Ω R5 → LED D2 → GND

System LED (Blue) D3:
ESP32-C3 GPIO19 → 330Ω R8 → LED D3 → GND
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

## PCB Layout Considerations (Updated)

### Signal Routing Priority
1. **Power Distribution**: Wide traces for 5V and 3.3V rails
2. **CAN Differential Pair**: 120Ω impedance, length matched (GPIO6/7)
3. **I2C Lines**: Keep traces short, add test points (GPIO4/5)
4. **SPI Interface**: Route as group (GPIO8/9/10)
5. **Sensor Ports**: Multi-function GPIO0-3 with protection

### Ground Plane Strategy
```
Solid ground plane on bottom layer
Via stitching for thermal and electrical connection
Separate analog/digital ground areas if needed
Ground guards around crystal oscillator
Star grounding for sensitive analog sensors
```

### Component Placement (Updated)
```
Layout (Top View):
[RJ45] [ESP32-C3] [TJA1050] [AMS1117]
  |       |         |         |
[LEDs]  [Crystal]  [Decoup]   [Power]
  |       |         |         |
[J3]    [J4-J7]   [Jumpers]  [J2]
SPI     Sensors   Config     Prog
```

## Hardware Compatibility Matrix

### Firmware Sensor Types Supported
| Sensor Type | Interface | GPIOs Used | Additional Hardware |
|-------------|-----------|------------|---------------------|
| Generic 4-port | GPIO0-3 | 4 | Pull-ups, protection |
| BME280 | SPI | GPIO8-10 | CS selection |
| MAX31855 | SPI | GPIO8-10 | CS selection |
| ADS1115 | I2C | GPIO4-5 | Pull-ups, addressing |

### Pin Sharing Strategy
```
GPIO9: Boot button (programming) / SPI_MISO (operation)
GPIO21: UART_TX (programming) / CAN_SILENT (operation)
GPIO0-3: Multi-function sensor ports with jumper configuration
```

## Design Validation Checklist (Updated)

### Electrical Verification
- [ ] 5V input protection circuit sized correctly
- [ ] 3.3V regulation stable under load
- [ ] CAN differential impedance = 120Ω ±10%
- [ ] I2C pull-up values appropriate for bus loading
- [ ] SPI signal integrity maintained at 10MHz
- [ ] All GPIOs within ESP32-C3 electrical limits
- [ ] Sensor port protection adequate for 5V inputs

### Signal Integrity
- [ ] CAN-H/CAN-L routed as differential pair (GPIO6/7)
- [ ] I2C traces < 10cm for reliable operation
- [ ] SPI traces length-matched within 1mm
- [ ] Crystal traces < 5mm with ground guard
- [ ] Power supply ripple < 50mV p-p
- [ ] Sensor port crosstalk minimized

### Firmware Compatibility
- [ ] Pin assignments match firmware requirements
- [ ] All sensor types supportable with same hardware
- [ ] Programming interface accessible
- [ ] CAN bus configuration correct (500kbps)
- [ ] I2C addressing scheme compatible

### Manufacturing Considerations
- [ ] All components available in JLCPCB library
- [ ] SMD components suitable for automated assembly
- [ ] Through-hole components minimized
- [ ] Test points accessible for validation
- [ ] Jumper positions clearly marked

## Updated GPIO Summary

| GPIO | Primary Function | Secondary Function | Shared Usage |
|------|------------------|-------------------|--------------|
| GPIO0 | Sensor Port 0 | - | Multi-function |
| GPIO1 | Sensor Port 1 | - | Multi-function |
| GPIO2 | Sensor Port 2 | - | Multi-function |
| GPIO3 | Sensor Port 3 | - | Multi-function |
| GPIO4 | I2C_SDA | - | RJ45 Pin 5 |
| GPIO5 | I2C_SCL | - | RJ45 Pin 6 |
| GPIO6 | CAN_TX | - | TJA1050 TXD |
| GPIO7 | CAN_RX | - | TJA1050 RXD |
| GPIO8 | SPI_CLK | - | Expansion header |
| GPIO9 | SPI_MISO | BOOT button | Programming/Operation |
| GPIO10 | SPI_MOSI | - | Expansion header |
| GPIO18 | Status LED | - | - |
| GPIO19 | System LED | - | - |
| GPIO20 | UART_RX | - | Programming |
| GPIO21 | UART_TX | CAN_SILENT | Programming/Operation |

## Firmware Compatibility Notes

### Code Changes Required
The existing firmware will need these minimal pin definition updates:

```c
// Updated pin definitions for ac-sensor PCB
#define CAN_TX GPIO_NUM_6     // Changed from D6
#define CAN_RX GPIO_NUM_7     // Changed from D7

#define CLK GPIO_NUM_8        // Changed from D8
#define MISO GPIO_NUM_9       // Changed from D9
#define MOSI GPIO_NUM_10      // Changed from D10

// Sensor ports unchanged
#define SENSOR_PORT_0 GPIO_NUM_0  // D0 -> GPIO0
#define SENSOR_PORT_1 GPIO_NUM_1  // D1 -> GPIO1
#define SENSOR_PORT_2 GPIO_NUM_2  // D2 -> GPIO2
#define SENSOR_PORT_3 GPIO_NUM_3  // D3 -> GPIO3
```

### Configuration Compatibility
- **EEPROM structure**: No changes required
- **CAN protocol**: No changes required
- **I2C addressing**: No changes required
- **Sensor configuration**: No changes required

This updated design provides maximum compatibility with your existing firmware while optimizing for manufacturability and field deployment. The multi-function sensor ports and comprehensive interface support make this a very flexible platform for industrial and agricultural monitoring applications.
