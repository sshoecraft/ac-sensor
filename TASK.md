# AC-Sensor PCB - Task List

## Project: ac-sensor
**GitHub Repository**: ac-sensor  
**Description**: ESP32-C3 CAN Bus + I2C sensor board with custom RJ45 pinout

## Current Tasks (June 2, 2025)

### 🔄 Active Tasks
- [ ] **PCB Schematic Design** - Create KiCad schematic with custom RJ45 pinout
- [ ] **Component Selection** - Verify JLCPCB part availability and costs  
- [ ] **PCB Layout** - Route the PCB with CAN + I2C interfaces
- [ ] **Design Rule Check** - Validate against JLCPCB manufacturing specs
- [ ] **Generate Manufacturing Files** - Gerbers, drill files, BOM, CPL

### 📋 Completed Tasks
- [x] **Project Planning** - Analyzed reference design and requirements (2025-06-02)
- [x] **Prototype Analysis** - Updated design based on actual prototype image (2025-06-02)
- [x] **Component Selection Update** - Switched to terminal blocks, added boost converter (2025-06-02)

### ⚡ High Priority
1. Component verification for JLCPCB availability
2. Schematic capture and electrical validation
3. PCB layout with proper CAN bus impedance control

### 🔍 Discovered During Work
- Need to verify TJA1050T vs TJA1050 package availability
- Consider RJ45 connector with integrated magnetics vs separate
- Power supply design: determine if 5V boost needed for CAN transceiver
- EMI considerations for CAN differential signals

### 📝 Notes
- Reference design shows compact form factor with ESP32 module
- RJ45 connector appears to be standard 8P8C type
- Board dimensions approximately 80mm x 30mm
- Red enclosure suggests this is for industrial/prototyping use

### 🎯 Success Criteria
- [ ] PCB passes JLCPCB DRC checks
- [ ] All components available in JLCPCB parts library
- [ ] Manufacturing cost under $15 per board (qty 10)
- [ ] Board size matches reference design dimensions
- [ ] Proper CAN bus termination and signal integrity