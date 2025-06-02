# AC-Sensor Pinout Reference

## RJ45 Connector Pinout

### Physical Connector (8P8C)
```
  1 2 3 4 5 6 7 8
 ┌─┬─┬─┬─┬─┬─┬─┬─┐
 │ │ │ │ │ │ │ │ │
 └─┴─┴─┴─┴─┴─┴─┴─┘
```

### Signal Assignment
| Pin | Signal | Wire Color | Description |
|-----|--------|------------|-------------|
| 1 | +5VDC | Red | Power Input (5V DC) |
| 2 | GND | Black | Ground Reference |
| 3 | CAN-H | Blue | CAN Bus High |
| 4 | CAN-L | White | CAN Bus Low |
| 5 | SDA | Green | I2C Data Line |
| 6 | SCL | Yellow | I2C Clock Line |
| 7 | N/C | - | Not Connected |
| 8 | N/C | - | Not Connected |

## ESP32-C3 GPIO Assignment

### Primary Functions
| GPIO | Function | Direction | Description |
|------|----------|-----------|-------------|
| GPIO0 | CAN_TX | Output | CAN Transmit to TJA1050 |
| GPIO1 | CAN_RX | Input | CAN Receive from TJA1050 |
| GPIO2 | CAN_SILENT | Output | CAN Silent Mode Control |
| GPIO3 | STATUS_LED | Output | General Status LED |
| GPIO4 | I2C_SDA | Bi-dir | I2C Data (to RJ45 Pin 5) |
| GPIO5 | I2C_SCL | Output | I2C Clock (to RJ45 Pin 6) |
| GPIO8 | CAN_TX_LED | Output | CAN Transmit Activity LED |
| GPIO9 | BOOT_BTN | Input | Boot/Program Button |
| GPIO10 | CAN_RX_LED | Output | CAN Receive Activity LED |
| GPIO20 | UART_RX | Input | Programming UART RX |
| GPIO21 | UART_TX | Output | Programming UART TX |
| EN | RESET | Input | Reset Button Input |

### Power Pins
| Pin | Signal | Voltage | Description |
|-----|--------|---------|-------------|
| VCC | 3V3 | 3.3V | Main Power Supply |
| GND | Ground | 0V | Ground Reference |

## Programming Header (J2)

### 4-Pin Programming Interface
| Pin | Signal | Description |
|-----|--------|-------------|
| 1 | 3.3V | Power Supply |
| 2 | GND | Ground |
| 3 | TX | UART Transmit (GPIO21) |
| 4 | RX | UART Receive (GPIO20) |

### Programming Sequence
1. Connect USB-to-UART adapter
2. Hold BOOT button (GPIO9)
3. Press RESET button (EN)
4. Release RESET, then BOOT
5. Device enters download mode

## Status LEDs

### LED Indicators
| LED | Color | GPIO | Description |
|-----|-------|------|-------------|
| D1 | Red | Power | 5V Power Present |
| D2 | Green | GPIO3 | System Status |
| D3 | Yellow | GPIO8 | CAN TX Activity |
| D4 | Blue | GPIO10 | CAN RX Activity |

### LED States
- **Power (Red)**: Always on when 5V present
- **Status (Green)**: Controlled by firmware
- **CAN TX (Yellow)**: Blinks on CAN transmission
- **CAN RX (Blue)**: Blinks on CAN reception

## Jumper Configuration

### JP1 - CAN Termination
| Position | Description |
|----------|-------------|
| Open | No termination (default) |
| Closed | 120Ω termination enabled |

### JP2 - I2C Pull-up Voltage
| Position | Voltage | Use Case |
|----------|---------|----------|
| 1-2 | 3.3V | 3.3V I2C devices |
| 2-3 | 5.0V | 5V I2C devices |

## Protection Circuits

### Power Input Protection
- **F1**: 500mA Polyfuse (overcurrent protection)
- **D5**: TVS diode (transient protection)
- **C1**: 470µF input filter

### CAN Bus Protection
- **D6, D7**: TVS diodes on CAN-H/CAN-L
- **Common mode chokes** (optional)
- **120Ω termination** via JP1

## Cable Specifications

### Recommended Cable Types
- **Standard Ethernet**: CAT5e/CAT6 (twisted pairs)
- **Industrial**: Shielded twisted pair
- **Automotive**: CAN-specific cable (120Ω impedance)

### Maximum Cable Lengths
| Interface | Max Length | Notes |
|-----------|------------|-------|
| CAN Bus | 40m @ 1Mbps | Depends on bit rate |
| I2C | 10m @ 100kHz | With proper pull-ups |
| Power | 50m | Consider voltage drop |

## Electrical Specifications

### Power Requirements
- **Input Voltage**: 5V DC ±5%
- **Input Current**: 200mA maximum
- **Regulation**: 3.3V ±2% for ESP32-C3

### Signal Levels
- **CAN Bus**: Differential ±2V (TJA1050 standard)
- **I2C**: 3.3V or 5V logic levels
- **UART**: 3.3V logic levels

### Environmental Ratings
- **Operating Temperature**: -40°C to +85°C
- **Storage Temperature**: -55°C to +125°C
- **Humidity**: 5% to 95% non-condensing