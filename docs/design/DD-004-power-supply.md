# DD-004: Power Supply Architecture Selection

## Detailed Design Decision Analysis

**Document Number:** PH-BAL-DD-004-DETAIL  
**Project:** Industrial IoT pH Balancer  
**Company:** Cannasol Technologies  
**Author:** Development Team  
**Date:** June 28, 2025  
**Version:** 1.0  
**Status:** Decision Pending  

---

## Decision Statement

**Select the primary power supply architecture and backup power strategy for the Industrial IoT pH Balancer system.**

**Requirements:**

- **Primary Power:** 120V/240V AC mains power capability
- **System Voltage:** 12V DC system bus for pumps and peripherals
- **Logic Power:** 3.3V/5V for ESP32 and sensor circuits
- **Power Budget:** 50W maximum continuous operation
- **Backup Power:** 4-8 hours operation during power outages
- **Safety:** UL/CE compliant power supplies, isolation from mains
- **Environmental:** Outdoor-rated enclosures, IP65 protection
- **Efficiency:** >80% power conversion efficiency
- **Cost Target:** <$100 total power system per unit
- **Reliability:** 24/7 continuous operation capability
- **Monitoring:** Power status monitoring and low-voltage alerts

---

## Option Analysis

### Option A: AC/DC Power Supply with Battery Backup

#### Technical Specifications

**Primary AC/DC Power Supply**

**Core Components:**

- Switching power supply (120/240V AC to 12V DC)
- Battery backup system with charging circuit
- Power management and switching circuitry
- DC/DC converters for logic voltages (5V, 3.3V)
- Power monitoring and alert systems

**Performance Specifications:**

- **Input Voltage:** 90-264V AC (universal input)
- **Output Power:** 60W continuous (12V @ 5A)
- **Efficiency:** 85-90% typical
- **Regulation:** ±1% line and load regulation
- **Backup Power:** 12V 7-20Ah sealed lead acid or lithium battery
- **Backup Duration:** 4-12 hours depending on battery capacity
- **Charging:** Automatic float/absorption charging with temperature compensation
- **Logic Supplies:** 5V @ 2A, 3.3V @ 1A from DC/DC converters

**Available Products:**

- **Mean Well RS-75-12:** $25-35 (12V 6.2A switching supply)
- **UPS Battery Backup Module:** $40-80 (12V system with charging)
- **12V 7Ah SLA Battery:** $15-25
- **12V 20Ah LiFePO4 Battery:** $60-120

#### Cost Analysis

- **AC/DC Power Supply (60W):** $25-40
- **Battery Backup Controller:** $30-50
- **Backup Battery (7-20Ah):** $15-120
- **DC/DC Converters:** $10-20
- **Power Monitoring Circuit:** $5-15
- **Enclosure and Wiring:** $15-25
- **Total System Cost:** $100-270

#### Advantages

✅ **Proven Technology:** Standard UPS architecture widely used  
✅ **Long Backup Time:** 4-12 hours operation during outages  
✅ **Universal Input:** Works with worldwide AC voltage standards  
✅ **High Efficiency:** 85-90% power conversion efficiency  
✅ **Reliable Operation:** Automatic power source switching  
✅ **Expandable:** Battery capacity can be increased for longer backup  
✅ **Status Monitoring:** Can monitor AC power, battery voltage, and charging status  

#### Disadvantages

❌ **Higher Cost:** Exceeds budget by 0-170%  
❌ **Complex System:** Multiple power conversion stages  
❌ **Battery Maintenance:** SLA batteries require periodic replacement (3-5 years)  
❌ **Size/Weight:** Larger system footprint, heavier with batteries  
❌ **Heat Generation:** Power conversion generates heat requiring ventilation  

---

### Option B: DC Power Supply with Solar Charging

#### Technical Specifications

**Solar Power System**

**Core Components:**

- Solar panel array (50-100W capacity)
- MPPT charge controller
- Deep cycle battery bank (12V system)
- DC/DC converters for system voltages
- Power monitoring and load management

**Performance Specifications:**

- **Solar Input:** 50-100W solar panel (12V nominal)
- **Battery Storage:** 50-100Ah deep cycle batteries
- **System Voltage:** 12V DC bus
- **Backup Duration:** 3-7 days with proper sizing
- **Charging:** MPPT controller for optimal solar harvesting
- **Load Management:** Automatic load shedding during low battery conditions
- **Logic Supplies:** 5V @ 2A, 3.3V @ 1A from high-efficiency DC/DC converters

#### Cost Analysis

- **Solar Panel (75W):** $50-100
- **MPPT Charge Controller:** $30-60
- **Deep Cycle Battery (75Ah):** $80-150
- **DC/DC Converters:** $15-25
- **Power Monitoring System:** $20-40
- **Mounting and Wiring:** $25-50
- **Total System Cost:** $220-425

#### Advantages

✅ **Energy Independence:** No reliance on grid power  
✅ **Low Operating Cost:** No electricity costs after installation  
✅ **Environmental Benefits:** Clean renewable energy source  
✅ **Remote Installation:** Suitable for locations without AC power  
✅ **Long Backup:** Multi-day operation without external power  
✅ **Expandable:** Additional panels/batteries can be added  

#### Disadvantages

❌ **High Initial Cost:** Significantly exceeds budget  
❌ **Weather Dependent:** Performance varies with sunlight availability  
❌ **Complex Installation:** Requires solar panel mounting and positioning  
❌ **Seasonal Variation:** Reduced performance in winter months  
❌ **Battery Maintenance:** Deep cycle batteries require monitoring and replacement  
❌ **System Sizing:** Complex calculations required for reliable operation  

---

### Option C: Simple AC/DC Supply with Minimal Backup

#### Technical Specifications

**Basic Power Supply System**

**Core Components:**

- Compact switching power supply (12V output)
- Small backup battery (12V 7Ah SLA)
- Simple backup switching circuit
- Linear regulators for logic voltages
- Basic power monitoring

**Performance Specifications:**

- **Primary Supply:** 12V 5A switching power supply
- **Backup Battery:** 12V 7Ah sealed lead acid
- **Backup Duration:** 2-4 hours typical operation
- **Logic Voltages:** 5V and 3.3V from linear regulators
- **Efficiency:** 75-80% overall (including linear regulators)
- **Monitoring:** Basic AC power status and low battery warning

#### Cost Analysis

- **12V Switching Supply:** $15-25
- **Small SLA Battery:** $15-25
- **Backup Switching Circuit:** $10-20
- **Linear Regulators:** $5-10
- **Power Monitoring:** $5-10
- **Enclosure and Components:** $10-15
- **Total System Cost:** $60-105

#### Advantages

✅ **Budget Friendly:** Within or slightly above $100 target  
✅ **Simple Design:** Fewer components and complexity  
✅ **Reliable Operation:** Simple switching between AC and battery power  
✅ **Easy Maintenance:** Standard components, easy replacement  
✅ **Compact Size:** Smaller footprint than full UPS system  
✅ **Fast Implementation:** Minimal development time required  

#### Disadvantages

❌ **Limited Backup:** 2-4 hours backup time only  
❌ **Lower Efficiency:** Linear regulators waste power as heat  
❌ **Basic Monitoring:** Limited power status information  
❌ **Heat Issues:** Linear regulators require heat sinking  
❌ **Battery Life:** SLA batteries require replacement every 3-4 years  

---

### Option D: DC-Only System with External Power

#### Technical Specifications

**External DC Power Input System**

**Core Components:**

- DC input connector (12V-24V range)
- DC/DC buck/boost converter for voltage regulation
- Input protection and filtering
- Local energy storage (supercapacitors or small battery)
- Efficient switching regulators for all voltages

**Performance Specifications:**

- **Input Range:** 10-30V DC (customer-supplied power)
- **Input Protection:** Reverse polarity, overvoltage, overcurrent protection
- **Regulation:** 12V ±2% output regardless of input voltage
- **Logic Supplies:** High-efficiency switching regulators (>90%)
- **Backup:** Supercapacitor bank for brief outages (<10 minutes)
- **Power:** 30-50W depending on load requirements

#### Cost Analysis

- **DC/DC Converter Module:** $15-25
- **Input Protection Circuit:** $5-15
- **Switching Regulators:** $10-20
- **Supercapacitor Backup:** $20-40
- **Connectors and Wiring:** $5-15
- **Total System Cost:** $55-115

#### Advantages

✅ **Lowest Cost:** Minimal power conversion required  
✅ **High Efficiency:** >90% conversion efficiency possible  
✅ **Flexible Input:** Wide input voltage range accommodation  
✅ **No Battery Maintenance:** Supercapacitors have 10+ year life  
✅ **Compact Design:** Smallest possible power system  
✅ **Fast Backup:** Instant response to power interruptions  

#### Disadvantages

❌ **External Dependency:** Requires customer-provided DC power source  
❌ **Limited Backup:** Very short backup time (minutes only)  
❌ **Installation Complexity:** Customer must provide appropriate DC supply  
❌ **Voltage Compatibility:** Must work with various customer power systems  
❌ **Safety Concerns:** Customer-supplied power may not meet safety standards  

---

## Comparative Analysis

### Performance Comparison

| Criterion | AC/DC with Battery (A) | Solar Power (B) | Simple AC/DC (C) | DC-Only External (D) |
|-----------|------------------------|-----------------|------------------|---------------------|
| **Backup Duration** | 4-12 hours | 3-7 days | 2-4 hours | <10 minutes |
| **Power Efficiency** | 85-90% | 80-85% | 75-80% | >90% |
| **Installation Complexity** | Moderate | High | Low | Low |
| **Maintenance Requirements** | Battery replacement (3-5 years) | Battery/panel maintenance | Battery replacement (3-4 years) | Minimal |
| **Environmental Suitability** | Indoor/outdoor | Outdoor required | Indoor/outdoor | Indoor/outdoor |
| **Grid Independence** | No | Yes | No | Depends on customer |
| **Scalability** | Good | Excellent | Limited | Good |
| **Safety Compliance** | Excellent (UL listed) | Good | Good | Depends on input source |

### Cost Comparison

| Component | Option A | Option B | Option C | Option D |
|-----------|----------|----------|----------|----------|
| **Primary Power Supply** | $25-40 | $50-100 | $15-25 | $15-25 |
| **Backup/Storage System** | $45-170 | $110-210 | $15-25 | $20-40 |
| **Power Management** | $15-35 | $50-100 | $15-25 | $10-30 |
| **Monitoring/Protection** | $5-15 | $20-40 | $5-10 | $5-15 |
| **Installation Hardware** | $15-25 | $25-50 | $10-15 | $5-15 |
| **Total Cost** | $105-285 | $255-500 | $60-100 | $55-125 |
| **vs. Budget ($100)** | +5-185% | +155-400% | -40-0% | -45-25% |

### Reliability and Safety Analysis

| Aspect | Option A | Option B | Option C | Option D |
|--------|----------|----------|----------|----------|
| **Power Availability** | Excellent | Excellent | Good | Fair |
| **Component Reliability** | Good | Fair (weather dependent) | Good | Excellent |
| **Safety Standards** | Excellent | Good | Good | Variable |
| **Fire/Shock Hazards** | Low (UL listed) | Low | Low | Depends on input |
| **Lightning Protection** | Standard AC protection | Enhanced protection needed | Standard AC protection | Customer dependent |
| **Failure Mode Analysis** | Graceful degradation | Weather/seasonal issues | Limited backup | External power dependent |

---

## Decision Matrix Analysis

### Scoring Criteria (1-10 scale, 10 = best)

| Criterion | Weight | Option A | Option B | Option C | Option D |
|-----------|--------|----------|----------|----------|----------|
| **Cost** | 30% | 6 (+5-185% budget) | 3 (+155-400% budget) | 9 (-40-0% budget) | 9 (-45-25% budget) |
| **Backup Duration** | 25% | 9 (4-12 hours) | 10 (days) | 6 (2-4 hours) | 2 (<10 minutes) |
| **Reliability** | 20% | 8 (proven UPS) | 6 (weather dependent) | 7 (simple system) | 8 (high efficiency) |
| **Installation Ease** | 15% | 7 (moderate) | 4 (complex) | 9 (simple) | 8 (simple) |
| **Safety/Compliance** | 10% | 10 (UL listed) | 7 (good) | 8 (standard) | 6 (variable) |

### Weighted Scores

| Option | Calculation | **Total Score** |
|--------|-------------|-----------------|
| **Option A** | (6×0.30)+(9×0.25)+(8×0.20)+(7×0.15)+(10×0.10) | **7.60** |
| **Option B** | (3×0.30)+(10×0.25)+(6×0.20)+(4×0.15)+(7×0.10) | **5.80** |
| **Option C** | (9×0.30)+(6×0.25)+(7×0.20)+(9×0.15)+(8×0.10) | **7.85** |
| **Option D** | (9×0.30)+(2×0.25)+(8×0.20)+(8×0.15)+(6×0.10) | **6.60** |

---

## Final Decision

### Selected Option: Option C - Simple AC/DC Supply with Minimal Backup ✅ **RECOMMENDED**

**Decision Score:** 7.85/10 (highest rated option balancing cost and functionality)

**Final Configuration:**
- **Primary Power:** Mean Well LRS-50-12 (12V 4.2A) switching supply ($20)
- **Backup Battery:** 12V 7Ah sealed lead acid battery ($20)
- **Backup Switching:** Diode OR-ing with voltage monitoring ($15)
- **Logic Regulators:** LM2596 (5V) and AMS1117 (3.3V) switching/linear combo ($10)
- **Power Monitoring:** Voltage dividers and ESP32 ADC monitoring ($5)
- **Total System Cost:** $70-90 (within $100 budget)

**Key Decision Factors:**
1. **Budget Compliance:** $70-90 fits comfortably within $100 target
2. **Adequate Backup:** 2-4 hours sufficient for most power outages
3. **Simple Implementation:** Proven technology with minimal complexity
4. **Safety Compliance:** UL-listed power supplies ensure safety standards
5. **Maintenance Simplicity:** Standard components with clear replacement procedures

**Implementation Timeline:**
- **Week 1:** Component procurement and power supply testing
- **Week 2:** Battery backup integration and switching circuit development
- **Week 3:** Power monitoring implementation and ESP32 integration
- **Week 4:** Complete system testing and safety validation

---

## Recommendation

### Primary Recommendation: Option C - Simple AC/DC Supply with Minimal Backup ✅ **SELECTED**

**Justification for Pool/Spa Applications:**

1. **Appropriate Backup Duration:** 2-4 hours covers typical power outages; pH doesn't change rapidly in pools
2. **Cost-Effective Solution:** Provides essential functionality within budget constraints
3. **Reliable Components:** Standard switching power supplies have excellent reliability records
4. **Safety Compliance:** UL-listed components ensure electrical safety
5. **Simple Maintenance:** Battery replacement every 3-4 years is acceptable maintenance schedule

**Implementation Strategy:**

**Phase 1: Primary Power System (1 week)**
- Implement Mean Well LRS-50-12 switching power supply
- Design and test 5V and 3.3V regulator circuits
- Validate power quality and efficiency measurements
- Test electromagnetic compatibility and safety isolation

**Phase 2: Battery Backup Integration (1 week)**
- Integrate 12V 7Ah SLA battery with diode OR-ing
- Implement battery charging circuit with voltage regulation
- Test automatic switchover during AC power loss
- Validate backup duration under various load conditions

**Phase 3: Power Monitoring and Control (1 week)**
- Implement ESP32-based power monitoring (AC present, battery voltage)
- Develop low-voltage warning and shutdown procedures
- Create power status reporting for user interface
- Test complete power management system

### Alternative Recommendation: Option A (Premium Installations)

For commercial installations requiring extended backup power, Option A (AC/DC with Battery Backup) provides superior performance:

**Premium System Benefits:**
- 4-12 hours backup duration for extended outage protection
- Professional UPS-grade reliability and monitoring
- Expandable battery capacity for longer backup times
- Better power quality and regulation

**Cost-Performance Trade-off:**
- Cost increase: $105-285 vs. $70-90 (50-217% increase)
- Backup improvement: 4-12 hours vs. 2-4 hours (2-6x longer)
- Market justification: Commercial pools require higher reliability

### Power System Implementation Details

**ESP32 Power Monitoring:**
```cpp
class PowerMonitor {
private:
    const int AC_PRESENT_PIN = 34;
    const int BATTERY_VOLTAGE_PIN = 35;
    const float VOLTAGE_DIVIDER_RATIO = 4.3; // 12V -> 3.3V max
    
public:
    bool isACPresent() {
        return digitalRead(AC_PRESENT_PIN) == HIGH;
    }
    
    float getBatteryVoltage() {
        int adc_value = analogRead(BATTERY_VOLTAGE_PIN);
        float voltage = (adc_value * 3.3 / 4095.0) * VOLTAGE_DIVIDER_RATIO;
        return voltage;
    }
    
    bool isBatteryLow() {
        return getBatteryVoltage() < 11.0; // 11V threshold for 12V system
    }
    
    void checkPowerStatus() {
        if (!isACPresent() && isBatteryLow()) {
            // Implement emergency shutdown sequence
            ESP.deepSleep(0); // Minimal power consumption
        }
    }
};
```

**Battery Backup Circuit:**
```
AC Supply (12V) ──┬── Diode (1N5822) ──┬── System Load
                  │                    │
                  └── Charge Circuit   │
                                       │
SLA Battery (12V) ────── Diode (1N5822) ──┘
```

**Safety Considerations:**
- Use UL-listed power supplies for all AC-DC conversion
- Implement proper fusing on both AC input and DC outputs
- Ensure adequate ventilation for heat dissipation
- Follow NEC codes for electrical installation
- Include ground fault protection for outdoor installations

This implementation provides a reliable, cost-effective power system suitable for residential and light commercial pool/spa applications while maintaining upgrade paths for installations requiring extended backup power.