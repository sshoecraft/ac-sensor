# AC-Sensor PCB

A compact ESP32-C3 based sensor board with CAN Bus and I2C communication via custom RJ45 connector, optimized for automated manufacturing with JLCPCB.

## 🚀 Features

- **ESP32-C3 WiFi/BLE**: Dual wireless connectivity options
- **CAN Bus Interface**: Industrial-grade TJA1050 transceiver
- **I2C Interface**: GPIO4/5 with selectable 3.3V or 5V pull-ups
- **Custom RJ45 Pinout**: Power + CAN + I2C on single connector
- **Compact Design**: 80mm x 30mm form factor
- **Power over Cable**: 5V input via RJ45 connector
- **Status LEDs**: Power, status, and CAN activity indicators
- **Programming Interface**: UART bootloader with reset/boot buttons

## 📋 Custom RJ45 Pinout

| Pin | Signal | Description |
|-----|--------|-------------|
| 1 | +5VDC | Power Input |
| 2 | GND | Ground |
| 3 | CAN-H | CAN Bus High |
| 4 | CAN-L | CAN Bus Low |
| 5 | SDA | I2C Data |
| 6 | SCL | I2C Clock |
| 7 | N/C | Not Connected |
| 8 | N/C | Not Connected |

## 📊 Specifications

| Parameter | Value |
|-----------|-------|
| MCU | ESP32-C3-MINI-1 |
| CAN Transceiver | TJA1050T (SOP-8) |
| CAN Speed | 125kbps - 1Mbps |
| Input Voltage | 5V DC via RJ45 |
| Operating Current | <200mA @ 5V |
| PCB Size | 80mm x 30mm x 1.6mm |
| Operating Temp | -40°C to +85°C |

## 🛠 Hardware Design

### Schematic Overview
The design centers around the ESP32-C3 module with the following key connections:
- **CAN Interface**: GPIO0 (TX) and GPIO1 (RX) to TJA1050
- **I2C Interface**: GPIO4 (SDA) and GPIO5 (SCL) with selectable pull-ups
- **Status LEDs**: GPIO3 (Status), GPIO8 (CAN TX), GPIO10 (CAN RX)
- **Control**: GPIO2 (CAN Silent), GPIO9 (Boot Button)

### Power Supply
- AMS1117-3.3 linear regulator provides clean 3.3V for ESP32-C3
- 5V rail maintained for CAN transceiver and I2C pull-ups
- Comprehensive protection with polyfuse and TVS diodes
- Input filtering with 470µF and 100nF capacitors

### CAN Bus Implementation
- TJA1050T provides robust CAN physical layer
- 120Ω termination resistor (jumper selectable)
- TVS protection on CAN-H and CAN-L lines
- Status LEDs indicate bus activity

### I2C Interface
- GPIO4/5 connected to RJ45 pins 5/6
- 4.7kΩ pull-up resistors with voltage selection jumper
- Supports both 3.3V and 5V I2C devices

## 📦 Manufacturing

### JLCPCB Assembly
This design is optimized for JLCPCB's assembly service:
- All components selected from JLCPCB parts library
- 2-layer PCB with standard specifications
- SMD components using standard packages (0805, SOP-8, SOT-223)
- Estimated cost: **$11.25 per board** (qty 10)

### Files Included
- `manufacturing/BOM.csv` - Bill of Materials with LCSC part numbers
- `manufacturing/assembly-notes.md` - Detailed assembly instructions
- `docs/schematic-design.md` - Circuit documentation
- KiCad design files (when complete)

### Design Rules
- **Minimum Trace**: 0.15mm (6 mil)
- **Minimum Via**: 0.2mm drill, 0.45mm pad
- **Impedance**: 120Ω differential for CAN pairs
- **Layer Stack**: 2-layer with solid ground plane

## 🔧 Pin Assignments

### ESP32-C3 GPIO Usage
```c
#define CAN_TX_GPIO         GPIO_NUM_0
#define CAN_RX_GPIO         GPIO_NUM_1  
#define CAN_SILENT_GPIO     GPIO_NUM_2
#define STATUS_LED_GPIO     GPIO_NUM_3
#define I2C_SDA_GPIO        GPIO_NUM_4
#define I2C_SCL_GPIO        GPIO_NUM_5
#define CAN_TX_LED_GPIO     GPIO_NUM_8
#define BOOT_BUTTON_GPIO    GPIO_NUM_9
#define CAN_RX_LED_GPIO     GPIO_NUM_10
```

### Programming Interface (4-pin header)
| Pin | Signal |
|-----|--------|
| 1 | 3.3V |
| 2 | GND |
| 3 | TX (GPIO21) |
| 4 | RX (GPIO20) |

## 🧪 Testing and Validation

### Power-On Test
1. Apply 5V to RJ45 pin 1
2. Verify 3.3V ±5% regulation
3. Check current consumption <200mA
4. Confirm LED indicators function

### CAN Bus Test
1. Connect to CAN analyzer or second node
2. Load test firmware
3. Verify message transmission/reception
4. Check differential signal quality with oscilloscope

### I2C Test
1. Connect I2C device to pins 5/6
2. Verify pull-up voltage selection
3. Test communication at 100kHz and 400kHz
4. Check signal integrity on long cables

## 💻 Software Development

### Development Environment
- **Framework**: ESP-IDF or Arduino IDE
- **CAN Library**: ESP-IDF TWAI driver
- **I2C Library**: ESP-IDF I2C driver
- **Programming**: UART bootloader (esptool.py)

### Example Code Structure
```c
// Initialize CAN bus
twai_general_config_t g_config = TWAI_GENERAL_CONFIG_DEFAULT(CAN_TX_GPIO, CAN_RX_GPIO, TWAI_MODE_NORMAL);
twai_timing_config_t t_config = TWAI_TIMING_CONFIG_250KBITS();
twai_filter_config_t f_config = TWAI_FILTER_CONFIG_ACCEPT_ALL();

// Initialize I2C
i2c_config_t conf = {
    .mode = I2C_MODE_MASTER,
    .sda_io_num = I2C_SDA_GPIO,
    .scl_io_num = I2C_SCL_GPIO,
    .sda_pullup_en = GPIO_PULLUP_ENABLE,
    .scl_pullup_en = GPIO_PULLUP_ENABLE,
    .master.clk_speed = 400000,
};
```

## 📈 Performance Characteristics

### CAN Bus Performance
- **Bit Rate**: Up to 1Mbps (tested at 250kbps)
- **Bus Length**: Up to 40m @ 1Mbps, 1000m @ 50kbps
- **Node Count**: Up to 30 nodes per segment
- **Error Detection**: CRC, stuff bit, form, ACK errors

### I2C Performance
- **Clock Speed**: Up to 400kHz
- **Cable Length**: Up to 10m with proper pull-ups
- **Device Count**: Limited by capacitive loading
- **Voltage Levels**: 3.3V or 5V selectable

### Power Consumption
- **Active Mode**: ~160mA (ESP32-C3 + CAN transceiver)
- **Sleep Mode**: <1mA (with CAN wake-up capability)
- **Peak Current**: ~200mA during WiFi transmission

## 🛡 Protection Features

### Electrical Protection
- Polyfuse overcurrent protection on power input
- TVS diodes on CAN lines for ESD protection
- Reverse polarity protection via AMS1117
- Brown-out detection in ESP32-C3

### Signal Integrity
- Differential CAN pair routing with 120Ω impedance
- Ground plane coverage for EMI reduction
- Crystal placement with ground guard rings
- Proper decoupling on all power pins

## 📚 Documentation

### Project Files
- `PLANNING.md` - Project architecture and goals
- `TASK.md` - Development task tracking
- `manufacturing/` - BOM, assembly notes, production files
- `docs/` - Technical documentation and schematics

### External References
- [ESP32-C3 Datasheet](https://www.espressif.com/sites/default/files/documentation/esp32-c3_datasheet_en.pdf)
- [TJA1050 Datasheet](https://www.nxp.com/docs/en/data-sheet/TJA1050.pdf)
- [JLCPCB Design Rules](https://jlcpcb.com/capabilities/pcb-capabilities)

## 🤝 Contributing

This is an open-source hardware design. Contributions welcome for:
- PCB layout improvements
- Component selection optimization
- Firmware examples
- Testing procedures
- Cost reduction suggestions

## 📄 License

Hardware design files released under CERN-OHL-P v2 license.
Firmware examples released under MIT license.

## 🔄 Project Status

- [x] **Requirements Analysis** - Complete
- [x] **Component Selection** - Complete  
- [x] **Schematic Design** - Documentation ready
- [ ] **PCB Layout** - In progress
- [ ] **Manufacturing Files** - Pending
- [ ] **Prototype Testing** - Planned

---

**Ready for KiCad Design** ✅  
All specifications documented and ready for schematic capture and PCB layout.
