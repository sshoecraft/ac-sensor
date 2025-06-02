# Firmware Analysis and Pin Mapping Updates

## Key Findings from Firmware Review

### Current Firmware Configuration
- **CAN Bus**: Uses ESP32-TWAI-CAN library at 500kbps
- **Pin Mapping**: Currently using different pins than our PCB design
- **Sensor Types**: Multi-mode sensor with build-time configuration
- **Communication**: CAN Bus for data + configuration protocol

### Firmware Pin Usage (Current)
```c
// Current firmware pins (different from our PCB)
#define CAN_TX D6    // Currently GPIO6
#define CAN_RX D7    // Currently GPIO7

// SPI pins for additional sensors
#define MOSI D10     // Currently GPIO10
#define MISO D9      // Currently GPIO9  
#define CLK D8       // Currently GPIO8

// Generic sensor ports
sensors[] = {
  { D0 },  // GPIO0 - Flow/Temp/State/Relay
  { D1 },  // GPIO1 - Flow/Temp/State/Relay
  { D2 },  // GPIO2 - Flow/Temp/State/Relay
  { D3 },  // GPIO3 - Flow/Temp/State/Relay
};
```

### Our PCB Pin Mapping (Updated)
Based on firmware analysis, here's the optimal pin mapping:

```c
// Power and Ground
VCC  -> 3.3V
GND  -> Ground

// CAN Bus Interface (matches TJA1050 connections)
GPIO6  -> CAN_TX (TJA1050 Pin 1 - TXD)
GPIO7  -> CAN_RX (TJA1050 Pin 4 - RXD)
GPIO21 -> CAN_SILENT (TJA1050 Pin 8 - Silent mode)

// I2C Interface (RJ45 pins 5,6)
GPIO4  -> SDA (RJ45 Pin 5)
GPIO5  -> SCL (RJ45 Pin 6)

// SPI Interface (for expansion sensors)
GPIO8  -> SPI_CLK
GPIO9  -> SPI_MISO
GPIO10 -> SPI_MOSI

// Generic Sensor Ports (I2C addressable)
GPIO0  -> Sensor Port 1 (Flow/Temp/State/Relay)
GPIO1  -> Sensor Port 2 (Flow/Temp/State/Relay)
GPIO2  -> Sensor Port 3 (Flow/Temp/State/Relay)
GPIO3  -> Sensor Port 4 (Flow/Temp/State/Relay)

// Programming Interface
GPIO20 -> UART_RX (Programming)
GPIO21 -> UART_TX (Programming) - shared with CAN_SILENT
EN     -> RESET

// Status LEDs
GPIO18 -> Power LED
GPIO19 -> Status LED
```

## Firmware Sensor Types

The firmware supports multiple sensor configurations:

### 1. Generic Sensor (SENSOR_TYPE_GENERIC)
- **4-port configurable sensor board**
- **Port types**: Flow, Temperature (DS18B20), State, Relay
- **Flow sensors**: Pulse counting with PPL (Pulses Per Liter)
- **Temperature**: OneWire DS18B20 sensors
- **State**: Digital input monitoring
- **Relay**: Digital output control

### 2. BME280 Sensor (SENSOR_TYPE_BME)
- **Single environmental sensor**
- **Measurements**: Temperature, Humidity, Pressure
- **Interface**: SPI

### 3. MAX31855 Thermocouple (SENSOR_TYPE_MAX)
- **Single thermocouple interface**
- **Temperature range**: -200°C to +1350°C
- **Interface**: SPI

### 4. ADS1115 ADC (SENSOR_TYPE_ADS)
- **4-channel 16-bit ADC**
- **Configurable gain and data rate**
- **Interface**: I2C

## Configuration Protocol

### CAN Bus Protocol
- **Base ID**: 0x450-0x45E (15 sensor addresses)
- **Broadcast ID**: 0x45F (configuration/discovery)
- **Speed**: 500 kbps
- **Auto-configuration**: MAC-based unique ID generation

### Configuration Commands
```c
CONFIG_ACTION_GET_CONFIG   // Request current configuration
CONFIG_ACTION_SET_CONFIG   // Set sensor ID and port types
CONFIG_ACTION_SET_PPL      // Set flow sensor pulses per liter
CONFIG_ACTION_CLEAR_CONFIG // Reset to factory defaults
```

## Updated PCB Requirements

### Essential Changes Needed

1. **CAN Bus Pins**: 
   - Move from GPIO0/1 to GPIO6/7 to match firmware
   - This frees up GPIO0/1 for sensor ports

2. **SPI Interface Addition**:
   - Add SPI pins for expansion sensors (BME280, MAX31855)
   - GPIO8/9/10 for CLK/MISO/MOSI

3. **Generic Sensor Ports**:
   - GPIO0-3 need to support multiple functions:
     - Digital input (flow sensors, state monitoring)
     - OneWire interface (DS18B20 temperature)
     - Digital output (relay control)
   - Pull-up resistors on all ports

4. **I2C Compatibility**:
   - Current I2C pins (GPIO4/5) work well
   - Needed for ADS1115 and other I2C sensors

### Hardware Implications

1. **Multi-function GPIO Design**:
   ```
   GPIO0-3: Configurable sensor ports
   - Pull-up resistors (10kΩ)
   - ESD protection
   - 3.3V/5V level compatibility
   ```

2. **SPI Interface**:
   ```
   GPIO8: SPI_CLK
   GPIO9: SPI_MISO  
   GPIO10: SPI_MOSI
   - Standard SPI routing
   - Available on expansion header
   ```

3. **CAN Bus Update**:
   ```
   GPIO6: CAN_TX -> TJA1050 TXD
   GPIO7: CAN_RX -> TJA1050 RXD
   GPIO21: CAN_SILENT -> TJA1050 Silent
   ```

## RJ45 Pinout (Updated)

Our custom pinout still works perfectly:

| Pin | Signal | GPIO | Description |
|-----|--------|------|-------------|
| 1 | +5VDC | - | Power Input |
| 2 | GND | - | Ground |
| 3 | CAN-H | - | CAN Bus High |
| 4 | CAN-L | - | CAN Bus Low |
| 5 | SDA | GPIO4 | I2C Data |
| 6 | SCL | GPIO5 | I2C Clock |
| 7 | N/C | - | Not Connected |
| 8 | N/C | - | Not Connected |

## Firmware Compatibility Notes

### Current Code Changes Needed
The existing firmware will need minimal changes for our PCB:

```c
// Update pin definitions for our PCB
#define CAN_TX GPIO_NUM_6    // Was D6
#define CAN_RX GPIO_NUM_7    // Was D7
#define CAN_SILENT GPIO_NUM_21

// SPI pins 
#define CLK GPIO_NUM_8       // Was D8
#define MISO GPIO_NUM_9      // Was D9
#define MOSI GPIO_NUM_10     // Was D10

// Sensor ports (unchanged)
#define SENSOR_PORT_0 GPIO_NUM_0
#define SENSOR_PORT_1 GPIO_NUM_1
#define SENSOR_PORT_2 GPIO_NUM_2
#define SENSOR_PORT_3 GPIO_NUM_3

// I2C (unchanged from our design)
#define I2C_SDA GPIO_NUM_4
#define I2C_SCL GPIO_NUM_5
```

### Configuration EEPROM
The firmware stores configuration in EEPROM:
- **Sensor ID**: 0x450-0x45E
- **Port configurations**: Flow/Temp/State/Relay per port
- **Flow sensor PPL**: Pulses per liter calibration
- **Auto-discovery**: Via CAN broadcast

This design creates a very flexible sensor platform that can be configured for different applications without hardware changes, perfect for the industrial/agricultural monitoring shown in your prototype images.
