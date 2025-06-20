# JLCPCB Submission Package
## AC-Sensor PCB v1.2 - Production Ready

### 🎯 **STATUS: READY FOR MANUFACTURING** ✅

---

## 📦 Complete Manufacturing Package

### **1. PCB Fabrication Files**
Located in: `hardware/gerbers/`

| File | Description | Status |
|------|-------------|---------|
| `ac-sensor-F_Cu.gbr` | Top copper layer | ✅ Ready |
| `ac-sensor-B_Cu.gbr` | Bottom copper layer (Ground plane) | ✅ Ready |
| `ac-sensor-F_SilkS.gbr` | Top silkscreen | ✅ Ready |
| `ac-sensor-F_Mask.gbr` | Top solder mask | ✅ Ready |
| `ac-sensor-Edge_Cuts.gbr` | Board outline | ✅ Ready |
| `ac-sensor-PTH.drl` | Plated through holes | ✅ Ready |
| `ac-sensor-NPTH.drl` | Non-plated holes (mounting) | ✅ Ready |
| `gerber-job.gbrjob` | Fabrication job file | ✅ Ready |

### **2. Assembly Files**
Located in: `manufacturing/`

| File | Description | Status |
|------|-------------|---------|
| `BOM.csv` | Bill of Materials with LCSC part numbers | ✅ Ready |
| `CPL.csv` | Component Placement List | ✅ Ready |
| `assembly-notes.md` | Manufacturing instructions | ✅ Ready |

### **3. Design Documentation**
| File | Description | Status |
|------|-------------|---------|
| `hardware/design-rule-check.md` | DRC verification report | ✅ Passed |
| `docs/schematic-design.md` | Circuit documentation | ✅ Complete |
| `hardware/README.md` | Design specifications | ✅ Complete |

---

## 🏭 **JLCPCB Order Specifications**

### **PCB Specifications**
- **Board Size**: 80mm × 30mm
- **Layers**: 2-layer PCB
- **Thickness**: 1.6mm
- **Material**: FR4 TG130
- **Copper Weight**: 1oz (35μm)
- **Surface Finish**: HASL (Lead-free)
- **Solder Mask**: Green
- **Silkscreen**: White
- **Min Track/Spacing**: 6 mil (0.15mm)
- **Min Via Size**: 0.6mm / 0.3mm drill

### **Assembly Specifications**
- **Assembly Side**: Top side only
- **Component Count**: 20 SMD + 8 through-hole
- **Special Requirements**: None
- **Extended Parts**: ESP32-C3-MINI-1 (Extended library)
- **Basic Parts**: TJA1050, AMS1117, passives

---

## 💰 **Cost Estimation (Quantity 10)**

| Item | Cost per Board | Total (10 pcs) |
|------|---------------|----------------|
| **PCB Fabrication** | $1.50 | $15.00 |
| **Component Cost** | $6.25 | $62.50 |
| **Assembly Service** | $3.50 | $35.00 |
| **Extended Part Fee** | $3.00 | $3.00 (one-time) |
| **Shipping** | - | ~$15.00 |
| **TOTAL** | **~$11.25** | **~$130.50** |

*Note: Prices are estimates based on 2025 JLCPCB pricing*

---

## 📋 **Submission Checklist**

### **Files Verification** ✅
- [x] All Gerber files generated and validated
- [x] Drill files (PTH and NPTH) complete
- [x] BOM with verified LCSC part numbers
- [x] CPL with accurate component placement
- [x] Design rule check passed
- [x] Component availability confirmed

### **Technical Verification** ✅
- [x] Board size within JLCPCB limits (80×30mm)
- [x] All traces ≥0.15mm (exceeds 0.127mm minimum)
- [x] Via sizes ≥0.6mm diameter
- [x] Component footprints from JLCPCB library
- [x] CAN bus impedance control (120Ω differential)
- [x] Assembly-friendly component placement

### **Functional Verification** ✅
- [x] Schematic matches prototype implementation
- [x] GPIO assignments verified (D0-D3, D6-D7)
- [x] Power distribution adequate (5V for sensors)
- [x] JST connector layout balanced (2 per side)
- [x] Programming interface accessible

---

## 🚀 **Upload Instructions for JLCPCB**

### **Step 1: PCB Fabrication**
1. Go to [jlcpcb.com](https://jlcpcb.com)
2. Click "Quote Now"
3. Upload: `hardware/gerbers/` folder (zipped)
4. Verify specifications match table above
5. Select quantity: 10 pieces

### **Step 2: SMT Assembly**
1. Enable "SMT Assembly" 
2. Select "Assemble top side"
3. Upload BOM: `manufacturing/BOM.csv`
4. Upload CPL: `manufacturing/CPL.csv`
5. Confirm component placement in preview
6. Review and approve assembly cost

### **Step 3: Final Review**
1. Verify total cost (~$130 for 10 boards)
2. Check estimated delivery time
3. Add to cart and proceed to checkout

---

## ⚠️ **Important Notes**

### **Component Considerations**
- **ESP32-C3-MINI-1**: Extended part (+$3 setup fee)
- **TJA1050**: Basic part (no extra fees)
- **AMS1117-5.0**: Basic part (no extra fees)
- **Passives**: All 0805 basic parts

### **Assembly Notes**
- All SMD components on top side only
- Through-hole connectors require manual soldering
- RJ45 and JST connectors not included in assembly
- Programming header and buttons manual assembly

### **Quality Considerations**
- Request electrical testing for first batch
- Consider conformal coating for harsh environments
- Verify JST connector orientation before bulk order

---

## 📞 **Support Information**

**Design Files Location**: `/Users/steve/src/ac-sensor/`
**Last Updated**: June 20, 2025
**Design Revision**: v1.2
**Ready for Production**: ✅ **YES**

**Contact for Questions**: Check project documentation in `docs/` folder

---

## 🎉 **Project Complete!**

The AC-Sensor PCB design is now **production-ready** for JLCPCB manufacturing. All files have been generated, validated, and verified against JLCPCB's specifications.

**Next Step**: Upload files to JLCPCB and place order! 🚀