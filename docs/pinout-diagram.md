# AC-Sensor Pinout Reference (Updated from Firmware Analysis)

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

## ESP32-C3 GPIO Assignment (Updated from Firmware)

### CAN Bus Interface
| GPIO | Function | Direction | Description |
|------|----------|-----------|-------------|
| GPIO6 | CAN_TX | Output | CAN Transmit to TJA1050 TXD |
| GPIO7 | CAN_RX | Input | CAN Receive from TJA1050 RXD |
| GPIO21 | CAN_SILENT | Output | CAN Silent Mode Control |

### I2C Interface
| GPIO | Function | Direction | Description |
|------|----------|-----------|-------------|
| GPIO4 | I2C_SDA | Bi-dir | I2C Data (to RJ45 Pin 5) |
| GPIO5 | I2C_SCL | Output | I2C Clock (to RJ45 Pin 6) |

### SPI Interface (for expansion sensors)
| GPIO | Function | Direction | Description |
|------|----------|-----------|-------------|
| GPIO8 | SPI_CLK | Output | SPI Clock |
| GPIO9 | SPI_MISO | Input | SPI Master In Slave Out |
| GPIO10 | SPI_MOSI | Output | SPI Master Out Slave In |

### Generic Sensor Ports (Multi-function)
| GPIO | Function | Config | Description |
|------|----------|--------|-------------|
| GPIO0 | SENSOR_PORT_0 | Configurable | Flow/Temp/State/Relay |
| GPIO1 | SENSOR_PORT_1 | Configurable | Flow/Temp/State/Relay |
| GPIO2 | SENSOR_PORT_2 | Configurable | Flow/Temp/State/Relay |
| GPIO3 | SENSOR_PORT_3 | Configurable | Flow/Temp/State/Relay |

#### Sensor Port Configurations
- **Flow Sensor**: Digital input with interrupt (pulse counting)
- **Temperature (DS18B20)**: OneWire interface
- **State Monitor**: Digital input with pull-up
- **Relay Control**: Digital output

### Programming Interface
| GPIO | Function | Direction | Description |
|------|----------|-----------|-------------|
| GPIO20 | UART_RX | Input | Programming UART RX |
| GPIO21 | UART_TX | Output | Programming UART TX (shared with CAN_SILENT) |
| EN | RESET | Input | Reset Button Input |
| GPIO9 | BOOT | Input | Boot/Program Button (shared with SPI_MISO) |

### Status LEDs
| GPIO | Function | Color | Description |
|------|----------|-------|-------------|
| GPIO18 | POWER_LED | Red | 5V Power Present |
| GPIO19 | STATUS_LED | Green | System Status |

### Power Pins
| Pin | Signal | Voltage | Description |
|-----|--------|---------|-------------|
| VCC | 3V3 | 3.3V | Main Power Supply |
| GND | Ground | 0V | Ground Reference |

## Hardware Interfaces

### Programming Header (J2)
| Pin | Signal | GPIO | Description |
|-----|--------|------|-------------|
| 1 | 3.3V | - | Power Supply |
| 2 | GND | - | Ground |
| 3 | TX | GPIO21 | UART Transmit |
| 4 | RX | GPIO20 | UART Receive |

### SPI Expansion Header (J3)
| Pin | Signal | GPIO | Description |
|-----|--------|------|-------------|
| 1 | 3.3V | - | Power Supply |
| 2 | GND | - | Ground |
| 3 | CLK | GPIO8 | SPI Clock |
| 4 | MISO | GPIO9 | SPI Data In |
| 5 | MOSI | GPIO10 | SPI Data Out |
| 6 | CS | GPIO11 | SPI Chip Select |

### Generic Sensor Headers (J4-J7)
Each sensor port has a 3-pin header:
| Pin | Signal | Description |
|-----|--------|-------------|
| 1 | 3.3V | Power Supply |
| 2 | GPIO | Sensor Signal |
| 3 | GND | Ground |

## Sensor Type Configurations

### 1. Generic Multi-Port Sensor (Default)
- **4 configurable ports** (GPIO0-3)
- **Flow sensors**: YF-S201 (Pulses per liter configurable)
- **Temperature**: DS18B20 OneWire sensors
- **State monitoring**: Digital inputs
- **Relay control**: Digital outputs

### 2. BME280 Environmental Sensor
- **Single sensor via SPI**
- **Measurements**: Temperature, Humidity, Pressure
- **CS pin**: Configurable GPIO

### 3. MAX31855 Thermocouple
- **Single thermocouple via SPI**
- **Range**: -200°C to +1350°C
- **CS pin**: Configurable GPIO

### 4. ADS1115 4-Channel ADC
- **4-channel 16-bit ADC via I2C**
- **Address**: 0x48 (configurable)
- **Gain**: Configurable (6.144V to 0.256V)

## Communication Protocol

### CAN Bus Configuration
- **Speed**: 500 kbps
- **IDs**: 0x450-0x45E (15 sensors)
- **Broadcast**: 0x45F (configuration)
- **Auto-discovery**: MAC-based unique ID

### I2C Configuration
- **Speed**: 100kHz/400kHz
- **Pull-ups**: 4.7kΩ to 3.3V or 5V (jumper selectable)
- **Devices**: ADS1115, BME280, other I2C sensors

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

### JP3-JP6 - Sensor Port Pull-ups
| Position | Description |
|----------|-------------|
| Open | No pull-up (relay mode) |
| Closed | 10kΩ pull-up (input mode) |

## Programming Sequence
1. Connect USB-to-UART adapter to J2
2. Hold BOOT button (GPIO9)
3. Press RESET button (EN)
4. Release RESET, then BOOT
5. Device enters download mode
6. Use esptool.py or Arduino IDE

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
- **Standard Ethernet**: CAT5e/CAT6 (for short runs)
- **Industrial**: Shielded twisted pair
- **Outdoor**: UV-resistant, weatherproof

### Maximum Cable Lengths
| Interface | Max Length | Notes |
|-----------|------------|-------|
| CAN Bus | 40m @ 500kbps | Depends on bit rate |
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
- **SPI**: 3.3V logic levels
- **Sensor Ports**: 3.3V logic levels

### Environmental Ratings
- **Operating Temperature**: -40°C to +85°C
- **Storage Temperature**: -55°C to +125°C
- **Humidity**: 5% to 95% non-condensing

## Firmware Compatibility

The PCB design is optimized for the provided firmware with minimal code changes:

```c
// Pin definitions for ac-sensor PCB
#define CAN_TX GPIO_NUM_6     // Updated from D6
#define CAN_RX GPIO_NUM_7     // Updated from D7
#define CAN_SILENT GPIO_NUM_21

#define I2C_SDA GPIO_NUM_4
#define I2C_SCL GPIO_NUM_5

#define SPI_CLK GPIO_NUM_8    // Updated from D8
#define SPI_MISO GPIO_NUM_9   // Updated from D9
#define SPI_MOSI GPIO_NUM_10  // Updated from D10

// Sensor ports (unchanged from firmware)
#define SENSOR_PORT_0 GPIO_NUM_0  // D0 -> GPIO0
#define SENSOR_PORT_1 GPIO_NUM_1  // D1 -> GPIO1
#define SENSOR_PORT_2 GPIO_NUM_2  // D2 -> GPIO2
#define SENSOR_PORT_3 GPIO_NUM_3  // D3 -> GPIO3

// Programming interface
#define UART_TX GPIO_NUM_21
#define UART_RX GPIO_NUM_20
#define BOOT_BTN GPIO_NUM_9
```

## Design Notes

### Pin Sharing Strategy
- **GPIO21**: Shared between UART_TX and CAN_SILENT (mutually exclusive)
- **GPIO9**: Shared between BOOT button and SPI_MISO (programming vs operation)
- **GPIO0-3**: Multi-function sensor ports with configurable pull-ups

### Configuration Flexibility
The hardware supports all firmware sensor types without modification:
- **Generic mode**: Uses GPIO0-3 for various sensor types
- **BME280 mode**: Uses SPI interface (GPIO8-10)
- **MAX31855 mode**: Uses SPI interface (GPIO8-10)
- **ADS1115 mode**: Uses I2C interface (GPIO4-5)

This design provides maximum compatibility with your existing firmware while optimizing the PCB for manufacturing and field deployment.
