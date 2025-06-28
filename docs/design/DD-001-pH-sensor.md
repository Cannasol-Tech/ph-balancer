# DD-001: pH Sensor Technology Selection

## Detailed Design Decision Analysis

**Document Number:** PH-BAL-DD-001-DETAIL  
**Project:** Industrial IoT pH Balancer  
**Company:** Cannasol Technologies  
**Author:** Development Team  
**Date:** June 28, 2025  
**Version:** 1.0  
**Status:** Decision Made - Option A Selected  

---

## Decision Statement

**Select the primary pH measurement technology and specific sensor/probe combination for the Industrial IoT pH Balancer system with IN-LINE pH monitoring capability.**

**Requirements:**

- **Installation Type:** In-line flow-through measurement (NOT tank immersion)
- pH measurement range: 0-14 pH units
- Accuracy: ±0.01 pH units minimum (target: ±0.005 pH)
- Temperature compensation: Automatic or manual capability
- Response time: <30 seconds to 90% of final value in flowing conditions
- Interface: Compatible with ESP32-WROOM-UE microcontroller
- Environmental: Pool/spa chemical compatibility, continuous operation
- **Mounting:** NPT threaded fitting or insertion probe for pipe installation
- **Flow conditions:** Must operate in flowing water (0.5-5 GPM typical)
- **Pressure rating:** Minimum 50 PSI working pressure
- Cost target: <$350 total sensor cost per unit (updated for in-line capability)
- Reliability: 12+ months continuous operation

---

## Option Analysis

### Option A: Atlas Scientific EZO pH Kit with Flow-Through Cell

#### Technical Specifications

**Core Components:**

- EZO™ pH Circuit board with embedded microprocessor
- Flow-through pH probe or insertion probe with NPT threading
- Flow cell housing or tee fitting for pipe mounting
- Electrically isolated carrier board
- Pre-calibrated system with multiple calibration options

**Performance Specifications:**

- **Accuracy:** ±0.002 pH units (exceeds requirement)
- **Resolution:** 0.001 pH units
- **Range:** 0.001 to 14.000 pH
- **Response Time:** <10 seconds in flowing conditions
- **Flow Rate:** 0.1-10 GPM operating range
- **Pressure Rating:** 100 PSI maximum
- **Temperature Compensation:** Automatic with external temperature probe
- **Interface:** I2C (address 99, configurable), UART alternative available
- **Supply Voltage:** 3.3V or 5V compatible
- **Current Consumption:** <5mA typical

**Flow Cell Options:**

- **Atlas Scientific Flow Cell:** Custom flow-through housing with pH and temperature sensors
- **NPT Insertion Probe:** 1/2" or 3/4" NPT threaded insertion probe for tee fitting installation
- **Inline Flow Chamber:** Complete inline assembly with pipe connections

#### In-Line Installation Methods

```
Method 1: NPT Insertion Probe
    Pipe → Tee Fitting → NPT pH Probe → Continue Pipe
    
Method 2: Flow Cell Assembly  
    Pipe → Flow Cell (with pH sensor) → Continue Pipe
    
Method 3: Saddle/Clamp Installation
    Existing Pipe → Saddle Clamp → Insertion Probe
```

#### Cost Analysis (In-Line Configuration)

- **EZO pH Circuit:** $45-55
- **Flow-Through pH Probe:** $120-180 (specialized for flow applications)
- **Flow Cell/NPT Fittings:** $30-50
- **Calibration Solutions:** $15-25 (pH 4, 7, 10 buffers)
- **Installation Hardware:** $20-30 (tee fittings, pipe adapters)
- **Total System Cost:** $230-340

#### Integration with ESP32

```cpp
// Simple I2C integration example
#include <Wire.h>

class AtlasScientificpH {
private:
    static constexpr uint8_t I2C_ADDRESS = 99;
    static constexpr uint8_t READ_DELAY = 1000; // 1 second
    
public:
    bool initialize() {
        Wire.begin(21, 22); // ESP32 I2C pins
        Wire.beginTransmission(I2C_ADDRESS);
        return (Wire.endTransmission() == 0);
    }
    
    float readpH() {
        Wire.beginTransmission(I2C_ADDRESS);
        Wire.write("R");
        Wire.endTransmission();
        
        delay(READ_DELAY);
        
        Wire.requestFrom(I2C_ADDRESS, 20);
        String response = "";
        while(Wire.available()) {
            response += (char)Wire.read();
        }
        
        return response.toFloat();
    }
};
```

#### Cost Analysis

- **EZO pH Circuit:** $45-55
- **pH Probe:** $60-80 (laboratory grade)
- **Calibration Solutions:** $15-25 (pH 4, 7, 10 buffers)
- **Total System Cost:** $120-160

#### Advantages

✅ **Exceptional Accuracy:** ±0.002 pH (5x better than requirement)  
✅ **Factory Calibrated:** Minimal setup and configuration required  
✅ **Digital Interface:** No analog signal conditioning needed  
✅ **Automatic Temperature Compensation:** With external temperature sensor  
✅ **Industrial Reliability:** Designed for continuous monitoring applications  
✅ **ESP32 Compatible:** Proven integration with ESP32 via I2C interface  
✅ **Comprehensive Library Support:** Arduino and ESP32 libraries available  
✅ **Electrical Isolation:** Built-in isolation prevents ground loop issues  

#### Disadvantages

❌ **Higher Cost:** Exceeds target cost by 20-60%  
❌ **Single Vendor Dependency:** Atlas Scientific exclusive technology  
❌ **Proprietary Protocol:** Custom commands, not standard sensor interface  
❌ **Complexity:** More sophisticated than needed for basic applications  
❌ **Probe Replacement Cost:** Expensive replacement probes ($60-80)  

---

### Option B: In-Line Analog pH Sensor with Signal Conditioning

#### Technical Specifications

**Core Components:**

- NPT threaded pH electrode with integrated reference (combination probe)
- High-impedance buffer amplifier (>1GΩ input impedance)  
- Temperature compensation circuit with flow-through temperature sensor
- Pressure-rated sensor housing
- ADC interface to ESP32

**Performance Specifications:**

- **Accuracy:** ±0.01 pH units (with proper calibration)
- **Resolution:** Limited by ADC (12-bit ESP32 = 0.003 pH theoretical)
- **Range:** 0-14 pH (full scale)
- **Response Time:** <30 seconds for 90% response in flowing conditions
- **Flow Rate:** 0.5-8 GPM operating range
- **Pressure Rating:** 150 PSI typical
- **Output:** mV signal (-414mV to +414mV for pH 0-14)
- **Temperature Coefficient:** -2mV/°C (requires compensation)
- **Interface:** Analog voltage to ESP32 ADC
- **Mounting:** 1/2" or 3/4" NPT threaded insertion

#### In-Line Mounting Options

```
Option B1: Tee Fitting Installation
    Main Pipe → Tee → NPT pH Probe → Ball Valve (for removal)
    
Option B2: Saddle Mount Installation  
    Existing Pipe → Hot Tap Saddle → Threaded pH Probe
    
Option B3: Inline Flow Chamber
    Pipe → Custom Flow Chamber → pH Probe + Temp Sensor → Pipe
```

#### Cost Analysis (In-Line Configuration)

**Option B1: Industrial NPT pH Probe**

- **NPT pH Electrode:** $80-120 (pressure rated, industrial grade)
- **Signal Conditioning PCB:** $20-30 (custom high-impedance design)
- **Flow-through Temperature Sensor:** $15-25
- **Installation Hardware:** $25-40 (tee fittings, ball valves)
- **Calibration Solutions:** $10-15
- **Total System Cost:** $150-230

**Option B2: Custom Flow Cell Design**

- **Standard pH Electrode:** $40-60
- **Custom Flow Cell Housing:** $30-50 (machined or 3D printed)
- **Signal Conditioning:** $20-30
- **Pressure Fittings:** $20-30
- **Temperature Integration:** $15-25
- **Total System Cost:** $125-195

#### Signal Conditioning Design

```
pH Electrode → High-Z Buffer → Level Shift → ESP32 ADC
(-414mV to +414mV) → (0V to 3.3V) → (0-4095 counts)

Theoretical Resolution: 3.3V / 4096 counts / 59mV/pH = 0.0014 pH/count
```

#### Integration with ESP32

```cpp
class AnalogpHSensor {
private:
    static constexpr uint8_t PH_ADC_PIN = 36;
    static constexpr float REFERENCE_VOLTAGE = 3.3;
    static constexpr float ADC_RESOLUTION = 4096.0;
    static constexpr float MV_PER_PH = 59.16; // At 25°C
    
    // Calibration parameters (determined during calibration)
    float ph7_voltage = 2.0; // Voltage at pH 7
    float slope = MV_PER_PH; // mV per pH unit
    
public:
    float readpH() {
        int adc_value = analogRead(PH_ADC_PIN);
        float voltage = (adc_value * REFERENCE_VOLTAGE) / ADC_RESOLUTION;
        
        // Convert voltage to pH using calibration
        float ph_value = 7.0 - ((voltage - ph7_voltage) * 1000.0 / slope);
        
        return ph_value;
    }
    
    void calibrate(float ph4_voltage, float ph7_voltage, float ph10_voltage) {
        // Two-point calibration using pH 4 and pH 10
        this->ph7_voltage = ph7_voltage;
        this->slope = ((ph10_voltage - ph4_voltage) * 1000.0) / (10.0 - 4.0);
    }
};
```

#### Cost Analysis

**Option B1: DFRobot Gravity pH Kit**

- **pH Electrode:** $25-35
- **Signal Conditioning Board:** $15-25
- **Calibration Solutions:** $10-15
- **Total System Cost:** $50-75

**Option B2: Custom Signal Conditioning**

- **pH Electrode:** $20-30
- **Op-amp + Components:** $5-10
- **PCB Space/Routing:** $5-10
- **Calibration Solutions:** $10-15
- **Total System Cost:** $40-65

#### Advantages

✅ **Lower Cost:** Meets target cost requirement  
✅ **Multiple Vendor Options:** Standard pH electrodes from many suppliers  
✅ **Replaceable Probes:** Standard BNC probes, lower replacement cost  
✅ **Customizable:** Full control over signal conditioning and calibration  
✅ **Standard Technology:** Well-understood pH measurement principles  
✅ **ESP32 Compatible:** Proven analog interface implementations  

#### Disadvantages

❌ **Lower Accuracy:** ±0.01 pH typical (at requirement limit)  
❌ **Calibration Complexity:** Manual calibration procedures required  
❌ **Temperature Sensitivity:** Requires temperature compensation implementation  
❌ **Signal Conditioning Required:** High-impedance analog design challenges  
❌ **Environmental Sensitivity:** Sensitive to moisture, electrical noise  
❌ **ADC Limitations:** ESP32 ADC accuracy affects overall system precision  

---

### Option C: Industrial In-Line pH Transmitter

#### Technical Specifications

**Core Components:**

- Industrial pH transmitter with NPT threaded sensor
- Standard pH electrode with pressure rating
- Integrated temperature compensation
- 4-20mA, 0-10V, or digital output options
- Explosion-proof and weatherproof ratings available

**Performance Specifications:**

- **Accuracy:** ±0.005 pH units
- **Resolution:** 0.001 pH units (digital output)
- **Range:** 0-14 pH
- **Response Time:** <15 seconds in flowing conditions
- **Flow Rate:** 0.1-15 GPM operating range  
- **Pressure Rating:** 150-300 PSI (depending on model)
- **Interface:** 4-20mA loop, 0-10V, MODBUS, or custom digital
- **Mounting:** 3/4" NPT insertion or flanged installation
- **Temperature Compensation:** Automatic with integrated PT100/1000

#### Example Products

**Sensorex pH Transmitter Series:**

- **Model:** pHT-4000 In-Line Transmitter
- **Accuracy:** ±0.005 pH
- **Outputs:** 4-20mA, 0-10V, HART protocol
- **Mounting:** 3/4" NPT insertion probe
- **Price Range:** $250-400

**Hach pH Sensors:**

- **Model:** PHC101 Process pH Sensor
- **Accuracy:** ±0.02 pH  
- **Outputs:** Analog or digital options
- **Mounting:** Insertion or flow-through cell
- **Price Range:** $300-500

#### Cost Analysis

- **Industrial pH Transmitter:** $250-400
- **Installation Hardware:** $30-50 (tee fittings, mounting)
- **Signal Interface Module:** $20-40 (4-20mA to ESP32 converter)
- **Calibration Equipment:** $15-25
- **Total System Cost:** $315-515

---

### Option D: Custom Flow-Through pH Cell

#### Technical Specifications

**Core Components:**

- Custom-designed flow cell with removable pH probe
- Standard BNC pH electrode (Atlas Scientific or similar)
- Integrated temperature sensor in flow stream
- 3D printed or machined flow cell housing
- Transparent flow cell for visual inspection

**Performance Specifications:**

- **Accuracy:** ±0.005-0.01 pH (depending on probe selection)
- **Resolution:** 0.001 pH units
- **Range:** 0-14 pH
- **Response Time:** <20 seconds (optimized flow design)
- **Flow Rate:** 0.5-5 GPM optimal range
- **Pressure Rating:** 50-100 PSI (depending on housing material)
- **Interface:** I2C (Atlas Scientific) or analog output
- **Mounting:** Standard pipe fittings (1/2" or 3/4")

#### Custom Flow Cell Design

```
Flow Cell Assembly:
    Inlet Fitting → Flow Chamber → pH Probe (removable) → Outlet Fitting
                        ↓
                Temperature Sensor (in flow)
                        ↓
                ESP32 Interface (I2C or Analog)
```

#### Design Advantages

✅ **Optimized Flow:** Custom flow pattern for fast response  
✅ **Probe Flexibility:** Use any standard BNC pH probe  
✅ **Cost Control:** Custom design optimized for application  
✅ **Maintenance Access:** Removable probe for cleaning/calibration  
✅ **Visual Confirmation:** Clear housing allows visual flow verification  

#### Cost Analysis

- **Atlas Scientific pH Probe:** $60-80
- **Custom Flow Cell Housing:** $40-60 (3D printed or machined)
- **Pipe Fittings and Valves:** $25-35
- **Temperature Sensor (DS18B20):** $5-10
- **Atlas Scientific EZO Circuit:** $45-55
- **Assembly and Testing:** $15-25
- **Total System Cost:** $190-265

#### Advantages

✅ **Good Accuracy:** ±0.005 pH (exceeds requirement)  
✅ **Probe Flexibility:** Use standard, lower-cost pH electrodes  
✅ **Digital Interface:** No analog signal conditioning needed  
✅ **Industrial Grade:** Designed for continuous monitoring  
✅ **Temperature Compensation:** Built-in automatic compensation  

#### Disadvantages

❌ **Highest Cost:** Significantly exceeds budget  
❌ **Complexity:** Additional component in signal chain  
❌ **Size:** Larger overall system footprint  
❌ **Power Consumption:** Higher than direct sensor options  

---

## Comparative Analysis

### Performance Comparison

| Criterion | Atlas Flow-Through (A) | Analog In-Line (B) | Industrial Transmitter (C) | Custom Flow Cell (D) |
|-----------|------------------------|-------------------|------------------------|---------------------|
| **Accuracy** | ±0.002 pH | ±0.01 pH | ±0.005 pH | ±0.005 pH |
| **Resolution** | 0.001 pH | 0.003 pH (ADC limited) | 0.001 pH | 0.001 pH |
| **Response Time** | <10 seconds | <30 seconds | <15 seconds | <20 seconds |
| **Flow Rate Range** | 0.1-10 GPM | 0.5-8 GPM | 0.1-15 GPM | 0.5-5 GPM |
| **Pressure Rating** | 100 PSI | 150 PSI | 150-300 PSI | 50-100 PSI |
| **Temperature Compensation** | Automatic | Manual/Software | Automatic | Automatic |
| **Installation Complexity** | Moderate | Moderate | Low (standard industrial) | High (custom) |
| **Maintenance Access** | Good | Good | Excellent | Excellent |

### Cost Comparison (In-Line Applications)

| Component | Option A | Option B1 | Option B2 | Option C | Option D |
|-----------|----------|-----------|-----------|----------|----------|
| **Sensor/Circuit** | $45-55 | $20-30 | $20-30 | $250-400 | $45-55 |
| **pH Probe** | $120-180 | $80-120 | $40-60 | Included | $60-80 |
| **Flow Hardware** | $30-50 | $25-40 | $30-50 | $30-50 | $40-60 |
| **Temperature Sensor** | Integrated | $15-25 | $15-25 | Integrated | $5-10 |
| **Installation** | $20-30 | $25-40 | $20-30 | $15-25 | $25-35 |
| **Interface Hardware** | $0 | $0 | $0 | $20-40 | $0 |
| **Total Cost** | $215-315 | $165-255 | $125-195 | $315-515 | $175-240 |
| **vs. Target ($350)** | -39-10% | -53-27% | -64-44% | -10-47% | -50-31% |

### Installation Complexity

| Aspect | Option A | Option B | Option C | Option D |
|--------|----------|----------|----------|----------|
| **Pipe Modifications** | Tee fitting required | Tee fitting required | Tee fitting required | Inline installation |
| **Pressure Testing** | Required | Required | Required | Required |
| **Calibration Access** | Moderate (probe removal) | Good (accessible probe) | Excellent (standard industrial) | Excellent (removable probe) |
| **Flow Disruption** | Minimal | Minimal | Minimal | None (inline design) |
| **Professional Install** | Recommended | Recommended | Required | Recommended |

---

## Decision Matrix Analysis

### Scoring Criteria (1-10 scale, 10 = best)

| Criterion | Weight | Option A | Option B1 | Option B2 | Option C | Option D |
|-----------|--------|----------|-----------|-----------|----------|----------|
| **Accuracy** | 25% | 10 (±0.002) | 6 (±0.01) | 6 (±0.01) | 8 (±0.005) | 8 (±0.005) |
| **Cost** | 25% | 9 (-10% budget) | 9 (-27% budget) | 10 (-44% budget) | 7 (-10% budget) | 9 (-31% budget) |
| **In-Line Suitability** | 20% | 7 (flow-through probe) | 8 (NPT threaded) | 8 (custom flow cell) | 9 (industrial standard) | 9 (optimized design) |
| **Integration Ease** | 15% | 9 (I2C library) | 5 (analog design) | 4 (custom design) | 6 (4-20mA interface) | 7 (I2C + custom) |
| **Reliability** | 10% | 9 (proven industrial) | 7 (pressure rated) | 6 (custom design) | 10 (industrial grade) | 7 (new design) |
| **Maintenance** | 5% | 7 (probe replacement) | 8 (accessible probe) | 8 (accessible probe) | 9 (standard industrial) | 9 (removable probe) |

### Weighted Scores

| Option | Calculation | **Total Score** |
|--------|-------------|-----------------|
| **Option A** | (10×0.25)+(9×0.25)+(7×0.20)+(9×0.15)+(9×0.10)+(7×0.05) | **8.43** |
| **Option B1** | (6×0.25)+(9×0.25)+(8×0.20)+(5×0.15)+(7×0.10)+(8×0.05) | **7.40** |
| **Option B2** | (6×0.25)+(10×0.25)+(8×0.20)+(4×0.15)+(6×0.10)+(8×0.05) | **7.40** |
| **Option C** | (8×0.25)+(7×0.25)+(9×0.20)+(6×0.15)+(10×0.10)+(9×0.05) | **8.10** |
| **Option D** | (8×0.25)+(9×0.25)+(9×0.20)+(7×0.15)+(7×0.10)+(9×0.05) | **8.20** |

---

## Final Decision

### Selected Option: Option A - Atlas Scientific EZO pH Kit with Flow-Through Cell

**Decision Date:** June 28, 2025  
**Decision Score:** 8.43/10 (highest rated option with $350 budget)

**Final Configuration:**
- **EZO pH Circuit:** $45-55 (I2C interface, factory calibrated)
- **Flow-Through pH Probe:** $120-180 (NPT threaded for inline installation)
- **Flow Cell/NPT Fittings:** $30-50 (tee fitting or flow cell assembly)
- **Calibration Solutions:** $15-25 (pH 4, 7, 10 buffers)
- **Installation Hardware:** $20-30 (tee fittings, pipe adapters)
- **Total System Cost:** $230-340 (within $350 budget)

**Key Decision Factors:**
1. **Exceptional Accuracy:** ±0.002 pH (5x better than requirement)
2. **Budget Compliance:** $230-340 fits within updated $350 target
3. **Proven Technology:** Factory calibrated, industrial reliability
4. **Fast Implementation:** No custom development required
5. **ESP32 Integration:** Established I2C interface and libraries

**Implementation Timeline:**
- **Week 1:** Component procurement and initial integration
- **Week 2-3:** ESP32 software development and testing
- **Week 4:** Installation testing and validation
- **Week 5:** Documentation and production procedures

---

## Recommendation

### Primary Recommendation: Option A - Atlas Scientific EZO pH Kit with Flow-Through Cell ✅ **SELECTED**

**Justification for In-Line Applications:**

1. **Optimal Design:** Custom flow cell optimized specifically for your application requirements
2. **Cost Balance:** Reasonable cost increase (17-60% over target) for in-line capability  
3. **Flexibility:** Uses standard Atlas Scientific probes with proven ESP32 integration
4. **Maintenance:** Excellent access for probe cleaning and replacement
5. **Performance:** Meets accuracy requirements with good response time

**Key Advantages for pH Balancer:**

- **Flow Optimization:** Custom flow pattern ensures fast, accurate pH measurement
- **Probe Standardization:** Uses readily available Atlas Scientific probes
- **Visual Confirmation:** Clear flow cell allows verification of proper operation
- **ESP32 Integration:** Proven I2C interface with comprehensive libraries
- **Modular Design:** Probe can be easily removed for calibration without system shutdown

### Implementation Strategy

**Phase 1: Flow Cell Design and Prototyping (3 weeks)**

- Design custom flow cell with CAD modeling
- 3D print prototype flow cells for testing
- Test flow patterns and response time optimization
- Validate pressure ratings and leak testing

**Phase 2: Integration and Testing (2 weeks)**  

- Integrate Atlas Scientific EZO pH circuit with ESP32
- Develop calibration procedures for flow-through operation
- Test with actual pool/spa chemicals in flowing conditions
- Validate accuracy against certified pH meter

**Phase 3: Production Design (2 weeks)**

- Finalize flow cell design for production manufacturing
- Select production materials (PETG, polycarbonate, or machined CPVC)
- Develop assembly and testing procedures
- Create installation documentation

### Alternative Recommendation: Option B2 (Cost-Optimized)

If budget constraints are critical, Option B2 (Custom Analog Flow Cell) provides acceptable performance at lowest cost:

**Implementation:**

- Custom flow cell with analog pH probe  
- Temperature compensation with DS18B20 in flow stream
- High-impedance analog conditioning on custom PCB
- Comprehensive calibration software with drift compensation

### Updated Cost Target

Given the in-line monitoring requirement, the cost target has been updated to **$350** to accommodate:

- Specialized flow-through sensors and mounting hardware
- Pressure ratings and chemical compatibility requirements  
- Installation complexity and professional mounting requirements
- Higher performance requirements for industrial applications

**Market Justification:** In-line pH monitoring provides significant value over tank probes:

- Real-time measurement of actual process conditions
- No lag time from sample mixing or stratification
- Professional installation appearance
- Reduced contamination and maintenance issues

This premium capability justifies the higher cost and positions the product competitively against industrial pH monitoring systems ($1,000-3,000+).

---

## Implementation Guide: Custom Flow-Through pH Cell

### Design Requirements Summary

Based on the analysis, the Custom Flow-Through pH Cell (Option D) provides the optimal balance of performance, cost, and functionality. Here's how to create it:

### 1. Flow Cell Housing Design

#### Mechanical Design Specifications

**Core Requirements:**

- **Material:** Chemical-resistant thermoplastic (PETG, Polycarbonate, or CPVC)
- **Pressure Rating:** 100 PSI minimum working pressure
- **Pipe Connections:** 1/2" or 3/4" NPT threaded inlet/outlet
- **pH Probe Port:** Standard BNC probe receptacle with O-ring seal
- **Temperature Sensor Port:** Waterproof fitting for DS18B20 sensor
- **Flow Pattern:** Optimized for <20 second response time
- **Visual Inspection:** Clear housing material for flow verification

#### CAD Design Features

```
Flow Cell Assembly (Top View):
    
    [NPT Inlet] → [Expansion Chamber] → [Sensor Zone] → [Contraction] → [NPT Outlet]
                        ↓                    ↓                ↓
                   Flow Distribution    pH Probe         Temperature 
                                      (removable)         Sensor
```

**Key Design Elements:**

1. **Expansion Chamber:** Reduces turbulence from inlet pipe
2. **Sensor Zone:** Laminar flow across pH probe for stable readings
3. **Probe Insertion Angle:** 45° insertion prevents air entrapment
4. **Temperature Integration:** DS18B20 positioned in main flow stream
5. **Drain Port:** Optional bottom drain for maintenance

#### 3D Printing Specifications

**Material Selection:**

- **Prototype:** PETG (chemical resistant, clear, easy to print)
- **Production Option 1:** Polycarbonate (higher strength, higher temperature)
- **Production Option 2:** Machined CPVC (highest chemical resistance)

**Print Settings (PETG):**

- **Layer Height:** 0.2mm for balance of speed and quality
- **Infill:** 100% for pressure applications
- **Wall Thickness:** 3-4 perimeters minimum
- **Support:** Required for overhangs, dissolvable preferred
- **Post-Processing:** Vapor smoothing for better sealing surfaces

### 2. Component Selection and Sourcing

#### Atlas Scientific EZO pH Circuit Integration

**Component List:**

```
Primary Components:
├── Atlas Scientific EZO pH Circuit (I2C) - $48
├── Atlas Scientific pH Probe (BNC connector) - $68
├── Waterproof DS18B20 Temperature Sensor - $8
├── Custom Flow Cell Housing - $45 (3D printed)
└── Pipe Fittings and Mounting Hardware - $30

Support Components:
├── BNC to Wire Adapter - $12
├── pH Calibration Solution Set (4, 7, 10) - $18
├── Isolation Board (optional) - $15
└── Mounting Brackets - $15
```

**Total System Cost:** $189-225 (within $350 target)

#### Alternative: Analog pH Probe Option

For cost optimization, Option B2 analog configuration:

```
Cost-Optimized Components:
├── Generic pH Probe (BNC) - $35
├── High-Impedance Buffer Op-Amp Circuit - $25
├── DS18B20 Temperature Sensor - $8
├── Custom Flow Cell Housing - $45
└── Pipe Fittings and Hardware - $30

Total Cost: $143 (30% under budget)
```

### 3. Flow Cell Manufacturing Process

#### Phase 1: Prototype Development (Week 1-2)

**Step 1: CAD Design**

- Create detailed 3D model in Fusion 360 or SolidWorks
- Design for 3D printing with minimal support material
- Include test ports for pressure testing
- Design modular probe mounting for different pH sensors

**Step 2: 3D Print and Test**

```bash
# Print settings example (for reference)
Material: PETG
Layer Height: 0.2mm
Print Speed: 50mm/s
Infill: 100%
Supports: Tree supports, 45° overhang threshold
```

**Step 3: Pressure Testing**

- Test to 150 PSI (1.5x working pressure)
- Use compressed air with soapy water leak detection
- Verify all threaded connections and O-ring seals
- Document any design modifications needed

#### Phase 2: Flow Optimization (Week 2-3)

**Flow Pattern Testing:**

```
Test Setup:
Water Pump → Flow Meter → Test Cell → pH Reference → Drain
                ↓            ↓           ↓
            Flow Rate    pH Response  Reference pH
            Measurement     Time      (calibrated meter)
```

**Optimization Targets:**

- **Response Time:** <20 seconds to 90% of final value
- **Flow Range:** Stable operation 0.5-5 GPM
- **Accuracy:** ±0.005 pH vs. reference meter
- **Repeatability:** <0.002 pH standard deviation

#### Phase 3: Electronics Integration (Week 3-4)

**ESP32 Interface Design:**

```cpp
// Complete pH monitoring system
#include <Wire.h>
#include <OneWire.h>
#include <DallasTemperature.h>

class FlowThroughpHSystem {
private:
    // Atlas Scientific EZO pH sensor
    static constexpr uint8_t EZO_I2C_ADDRESS = 99;
    
    // Temperature sensor
    OneWire oneWire;
    DallasTemperature tempSensor;
    
    // Calibration parameters
    float temperatureCoeff = -0.002; // pH/°C
    float referenceTemp = 25.0; // °C
    
public:
    bool initialize() {
        Wire.begin(21, 22); // ESP32 I2C pins
        tempSensor.begin();
        
        // Test EZO pH circuit communication
        Wire.beginTransmission(EZO_I2C_ADDRESS);
        return (Wire.endTransmission() == 0);
    }
    
    float readTemperature() {
        tempSensor.requestTemperatures();
        return tempSensor.getTempCByIndex(0);
    }
    
    float readpH() {
        // Read pH from EZO circuit
        Wire.beginTransmission(EZO_I2C_ADDRESS);
        Wire.write("R");
        Wire.endTransmission();
        
        delay(1000); // Wait for measurement
        
        Wire.requestFrom(EZO_I2C_ADDRESS, 20);
        String response = "";
        while(Wire.available()) {
            response += (char)Wire.read();
        }
        
        float pH = response.toFloat();
        float temperature = readTemperature();
        
        // Apply temperature compensation
        float tempCompensatedpH = pH + (temperatureCoeff * (temperature - referenceTemp));
        
        return tempCompensatedpH;
    }
    
    void calibratepH(float pH4_reading, float pH7_reading, float pH10_reading) {
        // Send calibration commands to EZO circuit
        String cal4_cmd = "Cal,mid," + String(pH7_reading, 3);
        String cal7_cmd = "Cal,low," + String(pH4_reading, 3);
        String cal10_cmd = "Cal,high," + String(pH10_reading, 3);
        
        // Implementation of calibration sequence
        // (details depend on EZO circuit command protocol)
    }
};
```

### 4. Assembly and Testing Procedures

#### Assembly Sequence

**Step 1: Housing Preparation**

1. Clean all 3D printed surfaces
2. Test-fit all components before final assembly
3. Apply thread sealant to NPT fittings
4. Install O-rings in pH probe and temperature sensor ports

**Step 2: Sensor Installation**

1. Insert DS18B20 temperature sensor through waterproof gland
2. Position sensor in main flow stream
3. Install pH probe in angled port with proper insertion depth
4. Secure all connections and verify no movement under pressure

**Step 3: Electronic Integration**

1. Connect Atlas Scientific EZO circuit to ESP32 via I2C
2. Connect DS18B20 to ESP32 digital pin with 4.7kΩ pull-up resistor
3. Install in weatherproof enclosure with cable glands
4. Test all connections before water testing

#### Validation Testing

**Accuracy Validation:**

```
Test Protocol:
1. Calibrate using pH 4, 7, 10 buffer solutions
2. Test against certified pH meter in flowing conditions
3. Record measurements every 30 seconds for 10 minutes
4. Calculate accuracy (bias) and precision (standard deviation)
5. Verify temperature compensation accuracy across 15-35°C range
```

**Flow Response Testing:**

```
Dynamic Response Test:
1. Establish steady flow at 2 GPM with pH 7 buffer
2. Inject pH 4 buffer upstream and record response time
3. Measure time to reach 90% of final pH reading
4. Repeat test with pH 10 buffer
5. Verify <20 second response time requirement
```

### 5. Production Considerations

#### Manufacturing Scale-Up

**Low Volume (1-100 units):**

- 3D print PETG housings in-house
- Manual assembly and testing
- Individual calibration certificates
- **Cost per unit:** $190-225

**Medium Volume (100-1000 units):**

- Injection molded polycarbonate housings
- Semi-automated assembly fixtures
- Batch calibration and testing
- **Cost per unit:** $145-175

**High Volume (1000+ units):**

- Injection molded CPVC housings
- Automated assembly and test stations
- Factory calibration with QR code tracking
- **Cost per unit:** $120-150

#### Quality Control

**Incoming Inspection:**

- Dimensional verification of 3D printed housings
- Electrical testing of Atlas Scientific circuits
- Calibration verification of pH probes

**Final Testing:**

- Pressure testing to 150 PSI
- Accuracy verification ±0.005 pH
- Response time verification <20 seconds
- Temperature compensation verification

This implementation guide provides a complete roadmap for creating the Custom Flow-Through pH Cell, from initial design through production scale-up.
