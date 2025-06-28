# DD-011: Flow Measurement Method

## Detailed Design Decision Analysis

**Document Number:** PH-BAL-DD-011-DETAIL  
**Project:** Industrial IoT pH Balancer  
**Company:** Cannasol Technologies  
**Author:** Development Team  
**Date:** June 28, 2025  
**Version:** 1.0  
**Status:** Decision Pending  

---

## Decision Statement

**Select flow measurement method for the Industrial IoT pH Balancer system to monitor water flow and detect circulation issues.**

**Requirements:**

- **Flow Range:** 0.5-15 GPM typical pool circulation rates
- **Accuracy:** ±10% adequate for flow detection and monitoring
- **Installation:** In-line or clamp-on installation in existing plumbing
- **Cost Target:** <$30 per flow sensor
- **Interface:** Compatible with ESP32 digital or analog inputs
- **Maintenance:** Minimal maintenance requirements
- **Chemical Compatibility:** Resistant to pool/spa chemicals
- **Response Time:** <30 seconds for flow detection
- **Pressure Rating:** Compatible with 50+ PSI pool systems
- **Purpose:** Flow detection, pump monitoring, leak detection

---

## Option Analysis

### Option A: Paddle Wheel Flow Sensor

#### Technical Specifications

**Insertion Paddle Wheel Flow Meter**

**Core Components:**
- Paddle wheel sensor with magnetic pickup
- NPT threaded housing for pipe installation
- Hall effect sensor for rotation detection
- Digital pulse output proportional to flow rate
- Chemical-resistant materials (PVC/CPVC body)

**Performance Specifications:**
- **Flow Range:** 1-20 GPM typical
- **Accuracy:** ±5% of full scale
- **Pulse Output:** 1-10 pulses per gallon
- **Pressure Rating:** 150 PSI maximum
- **Temperature Range:** 32-140°F
- **Installation:** 1/2" or 3/4" NPT threaded tee fitting

#### Cost Analysis
- **Paddle Wheel Sensor:** $15-25
- **Tee Fitting Installation:** $5-10
- **Signal Conditioning:** $3-8
- **Total System Cost:** $23-43

#### Advantages
✅ **Digital Output:** Clean digital pulses easy to interface  
✅ **Good Accuracy:** ±5% suitable for most applications  
✅ **Chemical Resistant:** PVC/CPVC materials handle pool chemicals  
✅ **Standard Installation:** NPT threading fits standard plumbing  

#### Disadvantages
❌ **Moving Parts:** Paddle wheel can stick or wear over time  
❌ **Minimum Flow:** Requires minimum flow rate to start spinning  
❌ **Installation Complexity:** Requires cutting into pipe system  

---

### Option B: Ultrasonic Flow Sensor

#### Technical Specifications

**Clamp-On Ultrasonic Flow Measurement**

**Core Components:**
- Ultrasonic transducers (transmit/receive pair)
- Clamp-on mounting system for existing pipes
- Digital signal processing for flow calculation
- Analog or digital output interface
- Weather-resistant electronics enclosure

**Performance Specifications:**
- **Flow Range:** 0.1-100 GPM depending on pipe size
- **Accuracy:** ±2% of reading
- **Installation:** Clamp-on, no pipe cutting required
- **Pipe Compatibility:** 1/2" to 6" pipe diameter
- **Output:** 4-20mA, 0-5V, or digital interface

#### Cost Analysis
- **Ultrasonic Flow Meter:** $150-400
- **Mounting Hardware:** $10-25
- **Signal Interface:** $5-15
- **Total System Cost:** $165-440

#### Advantages
✅ **Non-Invasive:** No pipe cutting or system modification  
✅ **High Accuracy:** ±2% reading accuracy  
✅ **No Moving Parts:** Ultrasonic technology very reliable  
✅ **Wide Range:** Works with various pipe sizes  

#### Disadvantages
❌ **High Cost:** Significantly exceeds $30 budget target  
❌ **Installation Complexity:** Requires precise sensor positioning  
❌ **Pipe Requirements:** Needs specific pipe materials and conditions  

---

### Option C: Simple Flow Switch

#### Technical Specifications

**Basic Flow Detection Switch**

**Core Components:**
- Flow-activated switch (reed switch or magnetic)
- Adjustable flow setpoint
- Simple on/off output
- Chemical-resistant wetted materials
- NPT threaded installation

**Performance Specifications:**
- **Function:** Flow/no-flow detection only
- **Setpoint Range:** 0.5-5 GPM activation
- **Output:** Simple on/off switch closure
- **Accuracy:** ±0.5 GPM setpoint accuracy
- **Installation:** 1/2" NPT threaded

#### Cost Analysis
- **Flow Switch:** $8-18
- **Installation Hardware:** $3-8
- **Interface Circuit:** $2-5
- **Total System Cost:** $13-31

#### Advantages
✅ **Low Cost:** Well within budget target  
✅ **Simple Interface:** Basic digital input to ESP32  
✅ **Reliable:** Simple switch mechanism  
✅ **Chemical Resistant:** Available in pool-compatible materials  

#### Disadvantages
❌ **Limited Information:** Flow/no-flow only, no rate measurement  
❌ **Fixed Setpoint:** Not adjustable once installed  
❌ **No Diagnostics:** Cannot detect partial flow restrictions  

---

### Option D: Venturi Flow Sensor

#### Technical Specifications

**Differential Pressure Flow Measurement**

**Core Components:**
- Venturi flow element in pipe
- Differential pressure sensor
- Signal conditioning electronics
- Analog output proportional to flow
- Pressure tap fittings

**Performance Specifications:**
- **Flow Range:** 2-30 GPM typical
- **Accuracy:** ±3% of full scale
- **Pressure Drop:** 2-5 PSI at maximum flow
- **Output:** 0-5V or 4-20mA analog
- **Installation:** Requires straight pipe runs

#### Cost Analysis
- **Venturi Element:** $25-45
- **Pressure Sensor:** $15-35
- **Signal Conditioning:** $8-20
- **Installation Hardware:** $5-15
- **Total System Cost:** $53-115

#### Advantages
✅ **No Moving Parts:** Very reliable operation  
✅ **Wide Flow Range:** Good turndown ratio  
✅ **Industrial Standard:** Proven technology  

#### Disadvantages
❌ **High Cost:** Exceeds budget significantly  
❌ **Pressure Drop:** Reduces system pressure  
❌ **Installation Requirements:** Needs straight pipe sections  

---

## Decision Matrix Analysis

### Scoring Criteria (1-10 scale, 10 = best)

| Criterion | Weight | Paddle Wheel (A) | Ultrasonic (B) | Flow Switch (C) | Venturi (D) |
|-----------|--------|------------------|----------------|-----------------|-------------|
| **Cost** | 35% | 8 (close to budget) | 3 (way over budget) | 10 (well under budget) | 4 (over budget) |
| **Reliability** | 25% | 6 (moving parts) | 9 (no moving parts) | 8 (simple switch) | 9 (no moving parts) |
| **Information Value** | 20% | 8 (flow rate) | 9 (precise flow rate) | 4 (flow/no-flow only) | 8 (flow rate) |
| **Installation Ease** | 15% | 6 (pipe cutting) | 8 (clamp-on) | 6 (pipe cutting) | 5 (complex install) |
| **Maintenance** | 5% | 6 (paddle wear) | 9 (no maintenance) | 8 (minimal) | 8 (minimal) |

### Weighted Scores

| Option | Calculation | **Total Score** |
|--------|-------------|-----------------|
| **Option A** | (8×0.35)+(6×0.25)+(8×0.20)+(6×0.15)+(6×0.05) | **7.20** |
| **Option B** | (3×0.35)+(9×0.25)+(9×0.20)+(8×0.15)+(9×0.05) | **6.40** |
| **Option C** | (10×0.35)+(8×0.25)+(4×0.20)+(6×0.15)+(8×0.05) | **7.70** |
| **Option D** | (4×0.35)+(9×0.25)+(8×0.20)+(5×0.15)+(8×0.05) | **6.40** |

---

## Final Decision

### Selected Option: Option C - Simple Flow Switch ✅ **RECOMMENDED**

**Decision Score:** 7.70/10 (highest rated balancing cost and functionality)

**Final Configuration:**
- **Flow Switch:** Adjustable flow switch with 1 GPM setpoint
- **Installation:** 1/2" NPT threaded in main circulation line
- **Output:** Normally open contacts to ESP32 digital input
- **Function:** Detect circulation pump operation and flow interruption
- **Total Cost:** $13-31 (well within $30 budget)

**Key Decision Factors:**
1. **Cost Effectiveness:** Well within budget at $13-31
2. **Appropriate Function:** Flow detection adequate for intended purpose
3. **Reliability:** Simple switch mechanism very reliable
4. **Easy Integration:** Simple digital input to ESP32
5. **Market Fit:** Adequate functionality for pool/spa monitoring

---

## Recommendation

### Primary Recommendation: Option C - Flow Switch ✅ **SELECTED**

**Implementation Strategy:**

Flow switch provides adequate functionality for pH balancer applications:
- Detect when circulation pump is running
- Alert when flow is interrupted (pump failure, valve closure)
- Prevent chemical injection when no flow present
- Simple integration requiring only digital input

**ESP32 Integration:**
```cpp
class FlowMonitor {
private:
    const int FLOW_SWITCH_PIN = 32;
    bool flow_detected = false;
    unsigned long no_flow_start = 0;
    const unsigned long NO_FLOW_TIMEOUT = 300000; // 5 minutes
    
public:
    void updateFlowStatus() {
        bool current_flow = digitalRead(FLOW_SWITCH_PIN) == HIGH;
        
        if (current_flow != flow_detected) {
            if (current_flow) {
                flow_detected = true;
                no_flow_start = 0;
            } else {
                flow_detected = false;
                no_flow_start = millis();
            }
        }
        
        // Check for extended no-flow condition
        if (!flow_detected && no_flow_start > 0) {
            if (millis() - no_flow_start > NO_FLOW_TIMEOUT) {
                triggerNoFlowAlarm();
            }
        }
    }
    
    bool isFlowOK() {
        return flow_detected;
    }
};
```

This flow switch approach provides essential flow monitoring while maintaining cost-effectiveness and simplicity appropriate for the pool/spa market.