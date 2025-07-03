# DD-002: Chemical Addition Method Selection

## Detailed Design Decision Analysis

**Document Number:** PH-BAL-DD-002-DETAIL  
**Project:** Industrial IoT pH Balancer  
**Company:** Cannasol Technologies  
**Author:** Development Team  
**Date:** June 28, 2025  
**Version:** 1.0  
**Status:** Decision Pending  

---

## Decision Statement

**Select the primary chemical addition method and hardware for acid/base chemical dosing in the Industrial IoT pH Balancer system.**

**Requirements:**

- **Chemical Types:** Muriatic acid (pH down) and sodium carbonate (pH up)
- **Dosing Accuracy:** ±5% minimum for effective pH control
- **Flow Rate Range:** 0.1-100 mL/min adjustable dosing capability
- **Chemical Compatibility:** Resistant to pool/spa chemicals and corrosive solutions
- **Control Interface:** Compatible with ESP32-WROOM-UE microcontroller
- **Environmental:** Outdoor installation capability, continuous operation
- **Safety:** Safe handling of corrosive chemicals, leak detection capability
- **Pressure Rating:** Compatible with 50+ PSI water systems
- **Cost Target:** <$200 total chemical addition system per unit
- **Reliability:** 12+ months continuous operation with minimal maintenance

---

## Option Analysis

### Option A: Peristaltic Pumps

#### Technical Specifications

**Large Peristaltic Dosing Pumps (12V DC)**

**Core Components:**

- Industrial peristaltic pump heads with motor drive
- Chemical-resistant tubing (Viton, PTFE, or Norprene)
- Pump controller with variable speed control
- Flow rate calibration and feedback system
- Chemical reservoirs with level sensing

**Performance Specifications:**

- **Flow Rate Range:** 0.1-100 mL/min continuously variable
- **Dosing Accuracy:** ±2% with calibration
- **Pressure Capability:** Up to 100 PSI back pressure
- **Tube Life:** 1-2 years continuous operation
- **Chemical Compatibility:** Excellent with proper tube selection
- **Power Requirements:** 12V DC, 2-5A depending on size
- **Control:** PWM speed control or stepper motor positioning

**Pump Options:**

- **Watson-Marlow 120U:** Industrial grade, $150-200 each
- **Cole-Parmer Masterflex:** Laboratory grade, $100-150 each
- **Kamoer NKP-DX:** Compact design, $80-120 each

#### Cost Analysis

- **Acid Dosing Pump:** $120-180
- **Base Dosing Pump:** $120-180
- **Pump Controllers (2x):** $40-80
- **Chemical Reservoirs (2x):** $30-60
- **Tubing and Fittings:** $25-40
- **Level Sensors (2x):** $20-40
- **Total System Cost:** $355-580

#### Advantages

✅ **High Accuracy:** ±2% dosing precision with proper calibration  
✅ **Self-Priming:** No external pressure source required  
✅ **Chemical Compatibility:** Excellent with proper tube materials  
✅ **Reversible Flow:** Can pump in either direction for flexible installation  
✅ **Proven Technology:** Well-established in chemical dosing applications  
✅ **Variable Flow:** Continuous flow rate adjustment via speed control  
✅ **No Check Valves:** Flow stops immediately when pump stops  

#### Disadvantages

❌ **Higher Cost:** Exceeds budget by 75-190%  
❌ **Tube Replacement:** Requires periodic tube replacement  
❌ **Mechanical Wear:** Moving parts subject to wear and failure  
❌ **Power Consumption:** Higher power requirements than alternatives  
❌ **Size:** Larger physical footprint  

---

### Option B: Solenoid Metering Valves

#### Technical Specifications

**Proportional Solenoid Valves**

**Core Components:**

- Proportional solenoid valve with electronic control
- Pressure regulated chemical supply system
- Flow rate feedback sensor
- Electronic valve controller
- Chemical reservoirs with pressurization system

**Performance Specifications:**

- **Flow Rate Range:** 0.01-10 mL pulse volume
- **Dosing Accuracy:** ±1% with pressure regulation
- **Response Time:** <100ms valve operation
- **Pressure Requirements:** 10-50 PSI supply pressure
- **Chemical Compatibility:** Depends on valve materials (PTFE/EPDM seals)
- **Power Requirements:** 12-24V DC, 0.5-2A peak
- **Control:** PWM or pulse width control

#### Cost Analysis

- **Proportional Valves (2x):** $80-120 each
- **Pressure Regulation System:** $60-100
- **Flow Sensors (2x):** $40-80
- **Chemical Reservoirs with Pressure:** $50-80
- **Electronic Controllers:** $30-50
- **Pressure Gauges and Safety:** $20-40
- **Total System Cost:** $360-590

#### Advantages

✅ **Very High Precision:** ±1% accuracy with proper pressure control  
✅ **No Wear Parts:** No tubes or mechanical components in contact with chemicals  
✅ **Fast Response:** Immediate flow control response  
✅ **Compact Size:** Smaller than peristaltic pumps  
✅ **Long Life:** No consumable parts in normal operation  

#### Disadvantages

❌ **Higher Cost:** Exceeds budget significantly  
❌ **Complex System:** Requires pressure regulation and feedback control  
❌ **Pressure Dependency:** Performance varies with supply pressure  
❌ **Chemical Compatibility:** Limited by valve materials  
❌ **Safety Concerns:** Pressurized chemical storage  

---

### Option C: Mini Peristaltic Pumps

#### Technical Specifications

**Small Peristaltic Pumps (5V/12V DC)**

**Core Components:**

- Compact peristaltic pump with DC motor
- Food-grade silicone or Viton tubing
- Simple on/off or PWM control
- Basic chemical reservoirs
- Manual or sensor-based level monitoring

**Performance Specifications:**

- **Flow Rate Range:** 0.1-10 mL/min variable
- **Dosing Accuracy:** ±5% typical
- **Pressure Capability:** Up to 20 PSI back pressure
- **Tube Life:** 6-12 months continuous operation
- **Chemical Compatibility:** Good with silicone tubing
- **Power Requirements:** 5-12V DC, 0.5-1A
- **Control:** Simple on/off relay or PWM

**Available Products:**

- **Generic Mini Peristaltic:** $15-25 each
- **Kamoer KPP-ST:** $30-40 each  
- **Adafruit Peristaltic Pump:** $25-35 each

#### Cost Analysis

- **Acid Dosing Pump:** $25-40
- **Base Dosing Pump:** $25-40
- **Pump Control Relays:** $10-20
- **Chemical Reservoirs:** $20-40
- **Tubing and Fittings:** $15-25
- **Level Indicators:** $10-20
- **Total System Cost:** $105-185

#### Advantages

✅ **Low Cost:** Well within budget target  
✅ **Simple Control:** Basic on/off or PWM control  
✅ **Compact Size:** Small footprint for tight installations  
✅ **Easy Replacement:** Low-cost pump replacement  
✅ **Self-Priming:** No external pressure required  
✅ **ESP32 Compatible:** Direct control capability  

#### Disadvantages

❌ **Lower Accuracy:** ±5% accuracy at requirement limit  
❌ **Limited Flow Range:** Lower maximum flow rates  
❌ **Shorter Tube Life:** More frequent maintenance required  
❌ **Pressure Limitations:** Limited back-pressure capability  
❌ **Chemical Compatibility:** Limited to basic tube materials  

---

### Option D: Gravity Feed with Control Valves

#### Technical Specifications

**Gravity Fed Chemical System**

**Core Components:**

- Elevated chemical reservoirs (2-3 feet above injection point)
- Solenoid control valves for flow control
- Flow rate calibration with timed pulses
- Level sensing for reservoir monitoring
- Manual flow rate adjustment valves

**Performance Specifications:**

- **Flow Rate Range:** Dependent on height and valve sizing
- **Dosing Accuracy:** ±10% with calibration and level compensation
- **Pressure Capability:** Gravity dependent (0.5-1 PSI per foot of height)
- **Reservoir Capacity:** Limited by mounting height constraints
- **Chemical Compatibility:** Depends on valve and reservoir materials
- **Power Requirements:** 12V DC solenoid valves, <1A
- **Control:** Timed pulse control with level compensation

#### Cost Analysis

- **Solenoid Valves (2x):** $20-40 each
- **Chemical Reservoirs with Mounting:** $40-80
- **Level Sensors (2x):** $15-30
- **Flow Control Valves:** $20-40
- **Mounting Hardware:** $25-50
- **Tubing and Fittings:** $15-25
- **Total System Cost:** $155-305

#### Advantages

✅ **Simple Design:** No pumps to fail or maintain  
✅ **Low Power:** Only solenoid valves require power  
✅ **Moderate Cost:** Within budget constraints  
✅ **Chemical Compatible:** Valve materials can be selected for compatibility  
✅ **Reliable Operation:** Fewer moving parts  

#### Disadvantages

❌ **Installation Constraints:** Requires elevated chemical storage  
❌ **Variable Pressure:** Flow rate changes with reservoir level  
❌ **Limited Accuracy:** ±10% accuracy below requirements  
❌ **Height Requirements:** May not be practical for all installations  
❌ **Flow Rate Limitations:** Lower flow rates than pump systems  

---

## Comparative Analysis

### Performance Comparison

| Criterion | Peristaltic Large (A) | Solenoid Valves (B) | Peristaltic Mini (C) | Gravity Feed (D) |
|-----------|----------------------|---------------------|---------------------|------------------|
| **Dosing Accuracy** | ±2% | ±1% | ±5% | ±10% |
| **Flow Rate Range** | 0.1-100 mL/min | 0.01-10 mL/pulse | 0.1-10 mL/min | Variable (limited) |
| **Chemical Compatibility** | Excellent | Good | Good | Good |
| **Pressure Capability** | 100 PSI | 50 PSI | 20 PSI | <2 PSI |
| **Power Consumption** | High (2-5A) | Medium (0.5-2A) | Low (0.5-1A) | Very Low (<1A) |
| **Maintenance Frequency** | Yearly tube replacement | Minimal | 6-month tube replacement | Minimal |
| **Installation Complexity** | Moderate | High | Low | Moderate |
| **System Reliability** | Good | Excellent | Fair | Good |

### Cost Comparison

| Component | Option A | Option B | Option C | Option D |
|-----------|----------|----------|----------|----------|
| **Primary Components** | $240-360 | $160-240 | $50-80 | $40-80 |
| **Control Systems** | $40-80 | $90-130 | $10-20 | $35-70 |
| **Reservoirs & Storage** | $30-60 | $50-80 | $20-40 | $40-80 |
| **Sensors & Monitoring** | $20-40 | $40-80 | $10-20 | $15-30 |
| **Installation Hardware** | $25-40 | $20-40 | $15-25 | $25-50 |
| **Total Cost** | $355-580 | $360-590 | $105-185 | $155-305 |
| **vs. Budget ($200)** | +78-190% | +80-195% | -48-8% | -23-53% |

### Safety and Reliability Analysis

| Aspect | Option A | Option B | Option C | Option D |
|--------|----------|----------|----------|----------|
| **Chemical Exposure Risk** | Low (enclosed tubing) | Medium (pressurized) | Low (enclosed tubing) | Low (gravity feed) |
| **Leak Detection** | Good (tube inspection) | Fair (pressure monitoring) | Good (tube inspection) | Excellent (visual) |
| **Failure Mode** | Tube rupture/wear | Valve failure | Tube rupture/wear | Valve sticking |
| **Emergency Shutdown** | Immediate (stop pump) | Fast (close valve) | Immediate (stop pump) | Fast (close valve) |
| **Maintenance Safety** | Good (tube replacement) | Complex (pressure system) | Good (easy access) | Excellent (atmospheric) |

---

## Decision Matrix Analysis

### Scoring Criteria (1-10 scale, 10 = best)

| Criterion | Weight | Option A | Option B | Option C | Option D |
|-----------|--------|----------|----------|----------|----------|
| **Dosing Accuracy** | 25% | 9 (±2%) | 10 (±1%) | 6 (±5%) | 4 (±10%) |
| **Cost** | 25% | 3 (+190% budget) | 3 (+195% budget) | 9 (-8% budget) | 7 (-23% budget) |
| **Chemical Compatibility** | 20% | 9 (excellent) | 7 (good) | 7 (good) | 7 (good) |
| **Reliability** | 15% | 7 (tube wear) | 9 (no wear parts) | 5 (frequent maintenance) | 8 (simple system) |
| **Installation Ease** | 10% | 6 (moderate complexity) | 4 (high complexity) | 8 (simple) | 6 (height constraints) |
| **Safety** | 5% | 8 (enclosed system) | 6 (pressurized) | 8 (enclosed system) | 9 (atmospheric) |

### Weighted Scores

| Option | Calculation | **Total Score** |
|--------|-------------|-----------------|
| **Option A** | (9×0.25)+(3×0.25)+(9×0.20)+(7×0.15)+(6×0.10)+(8×0.05) | **6.85** |
| **Option B** | (10×0.25)+(3×0.25)+(7×0.20)+(9×0.15)+(4×0.10)+(6×0.05) | **6.90** |
| **Option C** | (6×0.25)+(9×0.25)+(7×0.20)+(5×0.15)+(8×0.10)+(8×0.05) | **7.20** |
| **Option D** | (4×0.25)+(7×0.25)+(7×0.20)+(8×0.15)+(6×0.10)+(9×0.05) | **6.60** |

---

## Final Decision

### Selected Option: Option C - Mini Peristaltic Pumps ✅ **RECOMMENDED**

**Decision Score:** 7.20/10 (highest rated option within budget)

**Final Configuration:**
- **Acid Dosing Pump:** Kamoer KPP-ST ($35) with Viton tubing
- **Base Dosing Pump:** Kamoer KPP-ST ($35) with Viton tubing  
- **Pump Control:** Relay-based on/off control with PWM speed adjustment
- **Chemical Reservoirs:** Food-grade HDPE containers (1-2 gallon capacity)
- **Level Monitoring:** Float switches or ultrasonic sensors
- **Total System Cost:** $105-185 (within $200 budget)

**Key Decision Factors:**
1. **Budget Compliance:** $105-185 fits comfortably within $200 target
2. **Adequate Accuracy:** ±5% meets minimum requirement for pH control
3. **Simple Integration:** Direct ESP32 control via relays or PWM
4. **Proven Technology:** Established peristaltic pump technology
5. **Chemical Safety:** Enclosed tubing system minimizes exposure risk

**Implementation Timeline:**
- **Week 1:** Component procurement and pump testing
- **Week 2:** ESP32 integration and flow rate calibration
- **Week 3:** Chemical compatibility testing and reservoir design
- **Week 4:** System integration and safety validation

---

## Recommendation

### Primary Recommendation: Option C - Mini Peristaltic Pumps ✅ **SELECTED**

**Justification for Pool/Spa Applications:**

1. **Cost Effectiveness:** Provides necessary functionality within budget constraints
2. **Adequate Performance:** ±5% accuracy sufficient for pool/spa pH control (less critical than laboratory applications)
3. **Simple Maintenance:** Easy tube replacement procedure for end users
4. **Chemical Compatibility:** Viton tubing handles pool chemicals safely
5. **ESP32 Integration:** Direct control capability without additional interface hardware

**Implementation Strategy:**

**Phase 1: Pump Selection and Testing (1 week)**
- Procure Kamoer KPP-ST pumps for both acid and base
- Test flow rate accuracy and calibration procedures
- Validate chemical compatibility with pool chemicals
- Develop ESP32 control software

**Phase 2: System Integration (1 week)**
- Design chemical reservoir system with level monitoring
- Integrate flow rate calibration with pH feedback control
- Implement safety shutoffs and leak detection
- Test complete chemical dosing system

**Phase 3: Field Validation (1 week)**
- Install complete system in representative pool/spa environment
- Validate dosing accuracy under real-world conditions
- Test long-term reliability and tube life
- Document maintenance procedures

### Alternative Recommendation: Option A (Future Upgrade Path)

For premium installations where higher accuracy is required, Option A (Large Peristaltic Pumps) provides an upgrade path:

**Premium System Benefits:**
- ±2% dosing accuracy for critical applications
- Higher flow rates for large pool systems
- Extended tube life (1-2 years vs. 6-12 months)
- Better pressure handling for complex plumbing

**Cost-Performance Trade-off:**
- Cost increase: $355-580 vs. $105-185 (240-315% increase)
- Accuracy improvement: ±2% vs. ±5% (150% better)
- Market justification: Premium systems can support higher component costs

### Implementation Considerations

**Chemical Storage Safety:**
- Use secondary containment for all chemical reservoirs
- Install chemical-resistant materials throughout flow path
- Implement automatic shutoff on loss of prime or tube failure
- Provide clear visual indicators for chemical levels

**Flow Rate Calibration:**
- Develop automated calibration procedure using gravimetric method
- Implement temperature compensation for viscosity effects
- Store calibration data in ESP32 non-volatile memory
- Provide field recalibration capability

**Maintenance Protocol:**
- Monthly visual inspection of tubing for wear or chemical attack
- Semi-annual flow rate verification and recalibration
- Annual tube replacement (or as needed based on inspection)
- Chemical reservoir cleaning and inspection

This implementation provides a robust, cost-effective chemical dosing system suitable for the target market while maintaining upgrade paths for premium applications.