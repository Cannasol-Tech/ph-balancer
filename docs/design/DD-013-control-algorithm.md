# DD-013: Control Algorithm Approach

## Detailed Design Decision Analysis

**Document Number:** PH-BAL-DD-013-DETAIL  
**Project:** Industrial IoT pH Balancer  
**Company:** Cannasol Technologies  
**Author:** Development Team  
**Date:** June 28, 2025  
**Version:** 1.0  
**Status:** Decision Pending  

---

## Decision Statement

**Select the pH control algorithm and implementation strategy for the Industrial IoT pH Balancer system to achieve optimal pH control performance.**

**Requirements:**

- **Control Accuracy:** Maintain pH within ±0.1 pH units of setpoint
- **Settling Time:** Achieve setpoint within 30 minutes for typical disturbances
- **Overshoot:** Minimize pH overshoot to prevent damage or discomfort
- **Chemical Efficiency:** Minimize chemical consumption and waste
- **Robustness:** Stable operation across varying pool conditions
- **ESP32 Compatibility:** Algorithm must run efficiently on ESP32 microcontroller
- **Tuning Simplicity:** Minimal user configuration required
- **Disturbance Rejection:** Handle bather loads, rain, and chemical additions
- **Safety Integration:** Respect safety limits and pump constraints
- **Cost Impact:** Minimal computational overhead or additional hardware

---

## Option Analysis

### Option A: Bang-Bang Control with Adaptive Deadband

#### Technical Specifications

**Enhanced On/Off Control Algorithm**

**Core Components:**

- Adaptive deadband control with pH trend analysis
- Minimum pump runtime and off-time enforcement
- Progressive control with reduced deadband near setpoint
- Chemical dosing rate adaptation based on pH response
- Safety limit integration and pump protection

**Performance Specifications:**

- **Control Accuracy:** ±0.05-0.15 pH depending on system parameters
- **Deadband:** Variable deadband (±0.05 to ±0.2 pH)
- **Response Time:** 15-45 minutes depending on pool size and circulation
- **Overshoot:** Minimized through trend analysis and pump timing
- **Chemical Efficiency:** Good with proper deadband tuning
- **Computational Load:** Very low (<1% ESP32 CPU usage)

**Algorithm Implementation:**

```cpp
class AdaptiveBangBangController {
private:
    float setpoint = 7.4;
    float deadband_min = 0.05;
    float deadband_max = 0.15;
    float current_deadband = 0.1;
    
    unsigned long last_pump_time = 0;
    unsigned long min_pump_runtime = 30000; // 30 seconds
    unsigned long min_off_time = 120000;    // 2 minutes
    
    float ph_history[10];
    int history_index = 0;
    
public:
    void updateControl(float current_ph) {
        // Store pH history for trend analysis
        ph_history[history_index] = current_ph;
        history_index = (history_index + 1) % 10;
        
        float ph_error = current_ph - setpoint;
        float ph_trend = calculateTrend();
        
        // Adapt deadband based on error and trend
        adaptDeadband(abs(ph_error), ph_trend);
        
        // Control logic with safety checks
        if (ph_error > current_deadband) {
            if (canStartAcidPump()) {
                startAcidPump();
            }
        } else if (ph_error < -current_deadband) {
            if (canStartBasePump()) {
                startBasePump();
            }
        } else {
            stopAllPumps();
        }
    }
};
```

#### Cost Analysis

- **Software Development:** $500-1,000 (internal development)
- **Additional Hardware:** $0 (uses existing pumps and sensors)
- **Testing and Validation:** $200-500
- **Documentation:** $100-300
- **Total Development Cost:** $800-1,800

#### Advantages

✅ **Simple Implementation:** Easy to understand and implement  
✅ **Robust Operation:** Very stable, difficult to destabilize  
✅ **Low Computational Cost:** Minimal ESP32 processing requirements  
✅ **Easy Tuning:** Only deadband and timing parameters to adjust  
✅ **Proven Technology:** Well-understood control approach  
✅ **Fast Response:** Immediate response to pH deviations  
✅ **Chemical Compatible:** Works well with peristaltic pump systems  

#### Disadvantages

❌ **Control Oscillation:** Natural tendency to oscillate around setpoint  
❌ **Chemical Waste:** On/off operation may waste chemicals  
❌ **Pump Wear:** Frequent on/off cycling increases pump wear  
❌ **Limited Precision:** Difficulty achieving very tight control  

---

### Option B: PID Control with Anti-Windup

#### Technical Specifications

**Advanced PID Controller with Pool-Specific Optimizations**

**Core Components:**

- Proportional-Integral-Derivative control algorithm
- Anti-windup protection for integral term
- Gain scheduling based on pH error magnitude
- Derivative filtering to reduce noise sensitivity
- Output limiting and pump speed control

**Performance Specifications:**

- **Control Accuracy:** ±0.02-0.05 pH with proper tuning
- **Settling Time:** 20-40 minutes for step disturbances
- **Overshoot:** <0.1 pH with conservative tuning
- **Chemical Efficiency:** Excellent with continuous modulation
- **Computational Load:** Low-moderate (2-5% ESP32 CPU usage)
- **Tuning Parameters:** Kp, Ki, Kd gains plus limits and filters

**Algorithm Implementation:**

```cpp
class PoolPIDController {
private:
    float Kp = 2.0;    // Proportional gain
    float Ki = 0.1;    // Integral gain  
    float Kd = 0.5;    // Derivative gain
    
    float setpoint = 7.4;
    float integral = 0.0;
    float last_error = 0.0;
    float output_min = 0.0;
    float output_max = 100.0;
    
    unsigned long last_time = 0;
    
public:
    float updatePID(float current_ph) {
        unsigned long now = millis();
        float dt = (now - last_time) / 1000.0; // Convert to seconds
        
        if (dt <= 0) return 0; // Avoid division by zero
        
        float error = setpoint - current_ph;
        
        // Proportional term
        float proportional = Kp * error;
        
        // Integral term with windup protection
        integral += error * dt;
        if (integral > output_max / Ki) integral = output_max / Ki;
        if (integral < output_min / Ki) integral = output_min / Ki;
        float integral_term = Ki * integral;
        
        // Derivative term with filtering
        float derivative = Kd * (error - last_error) / dt;
        
        // Calculate output
        float output = proportional + integral_term + derivative;
        
        // Limit output
        if (output > output_max) output = output_max;
        if (output < output_min) output = output_min;
        
        // Store values for next iteration
        last_error = error;
        last_time = now;
        
        return output;
    }
};
```

#### Cost Analysis

- **Software Development:** $2,000-4,000 (complex algorithm implementation)
- **PWM Pump Control Hardware:** $20-40 (variable speed control)
- **Testing and Tuning:** $500-1,000 (extensive tuning required)
- **Documentation and Training:** $300-600
- **Total Development Cost:** $2,820-5,640

#### Advantages

✅ **Excellent Control Performance:** Very tight control possible (±0.02 pH)  
✅ **Smooth Operation:** Continuous control reduces oscillation  
✅ **Chemical Efficiency:** Optimal chemical usage with continuous modulation  
✅ **Industry Standard:** Well-established control approach  
✅ **Adaptive Capability:** Can be enhanced with gain scheduling  
✅ **Disturbance Rejection:** Excellent handling of load changes  

#### Disadvantages

❌ **Complex Tuning:** Requires expertise to tune properly  
❌ **Stability Concerns:** Can become unstable with poor tuning  
❌ **Hardware Requirements:** Needs variable speed pump control  
❌ **Higher Cost:** Additional development and hardware costs  
❌ **Computational Load:** More processing overhead than bang-bang  

---

### Option C: Fuzzy Logic Control

#### Technical Specifications

**Rule-Based Fuzzy Logic Controller**

**Core Components:**

- Fuzzy membership functions for pH error and rate of change
- Expert-knowledge rule base for control decisions
- Defuzzification to generate pump control signals
- Adaptive rule weighting based on system conditions
- Integration with safety and limit systems

**Performance Specifications:**

- **Control Accuracy:** ±0.03-0.08 pH depending on rule quality
- **Settling Time:** 25-50 minutes depending on rule optimization
- **Robustness:** Excellent handling of nonlinear behavior
- **Tuning Method:** Rule-based, intuitive for pool operators
- **Computational Load:** Moderate (5-10% ESP32 CPU usage)
- **Rules:** 15-25 fuzzy rules covering operating scenarios

**Algorithm Framework:**

```cpp
class FuzzyPoolController {
private:
    enum pHError { VERY_LOW, LOW, OK, HIGH, VERY_HIGH };
    enum pHRate { FALLING_FAST, FALLING, STABLE, RISING, RISING_FAST };
    enum PumpOutput { OFF, SLOW, MEDIUM, FAST };
    
    struct FuzzyRule {
        pHError error_state;
        pHRate rate_state;
        PumpOutput acid_output;
        PumpOutput base_output;
        float weight;
    };
    
    FuzzyRule rules[20] = {
        {VERY_HIGH, RISING_FAST, FAST, OFF, 1.0},
        {HIGH, RISING, MEDIUM, OFF, 0.8},
        {OK, STABLE, OFF, OFF, 1.0},
        // ... additional rules
    };
    
public:
    void updateFuzzyControl(float current_ph, float ph_rate) {
        // Fuzzification
        auto error_membership = calculateErrorMembership(current_ph);
        auto rate_membership = calculateRateMembership(ph_rate);
        
        // Rule evaluation
        float acid_output = 0, base_output = 0;
        for (auto& rule : rules) {
            float activation = min(error_membership[rule.error_state],
                                 rate_membership[rule.rate_state]) * rule.weight;
            
            acid_output = max(acid_output, activation * getOutputValue(rule.acid_output));
            base_output = max(base_output, activation * getOutputValue(rule.base_output));
        }
        
        // Apply control outputs
        setAcidPumpSpeed(acid_output);
        setBasePumpSpeed(base_output);
    }
};
```

#### Cost Analysis

- **Software Development:** $3,000-6,000 (complex rule development)
- **Rule Development and Testing:** $1,000-2,000
- **Expert Knowledge Integration:** $500-1,000
- **Variable Speed Control:** $20-40
- **Total Development Cost:** $4,520-9,040

#### Advantages

✅ **Intuitive Tuning:** Rules based on expert knowledge and common sense  
✅ **Robust Operation:** Handles nonlinear behavior and varying conditions well  
✅ **Smooth Control:** Natural handling of multiple operating modes  
✅ **Expert Knowledge Integration:** Can incorporate pool operator experience  
✅ **Fault Tolerance:** Graceful degradation with partial rule failures  

#### Disadvantages

❌ **High Development Cost:** Complex rule development and testing  
❌ **Computational Overhead:** Moderate processing requirements  
❌ **Rule Maintenance:** Rules may need updating as system changes  
❌ **Limited Precision:** May not achieve tightest possible control  

---

### Option D: Hybrid Adaptive Control

#### Technical Specifications

**Multi-Mode Adaptive Control System**

**Core Components:**

- Multiple control algorithms with automatic mode selection
- System identification for parameter estimation
- Adaptive switching between control modes
- Performance monitoring and optimization
- Learning capability for improved operation over time

**Performance Specifications:**

- **Control Accuracy:** ±0.02-0.1 pH depending on conditions and mode
- **Adaptation Time:** 1-7 days for system learning
- **Mode Selection:** Automatic based on system conditions
- **Control Modes:** Bang-bang, PID, and rule-based modes
- **Computational Load:** Variable (2-15% ESP32 CPU usage)

#### Cost Analysis

- **Software Development:** $5,000-10,000 (complex multi-algorithm system)
- **System Identification:** $1,000-2,000
- **Testing and Validation:** $1,500-3,000
- **Documentation:** $500-1,000
- **Total Development Cost:** $8,000-16,000

---

## Comparative Analysis

### Performance Comparison

| Criterion | Bang-Bang (A) | PID Control (B) | Fuzzy Logic (C) | Hybrid Adaptive (D) |
|-----------|---------------|-----------------|-----------------|-------------------|
| **Control Accuracy** | ±0.05-0.15 pH | ±0.02-0.05 pH | ±0.03-0.08 pH | ±0.02-0.1 pH |
| **Settling Time** | 15-45 minutes | 20-40 minutes | 25-50 minutes | 15-40 minutes |
| **Overshoot** | Moderate | Minimal | Low | Minimal |
| **Chemical Efficiency** | Good | Excellent | Good | Excellent |
| **Robustness** | Excellent | Good | Excellent | Excellent |
| **Tuning Difficulty** | Easy | Difficult | Moderate | Complex |
| **Computational Load** | Very Low | Low-Moderate | Moderate | Variable |
| **Implementation Time** | Fast | Moderate | Slow | Very Slow |

### Cost and Complexity Comparison

| Component | Option A | Option B | Option C | Option D |
|-----------|----------|----------|----------|----------|
| **Software Development** | $500-1,000 | $2,000-4,000 | $3,000-6,000 | $5,000-10,000 |
| **Additional Hardware** | $0 | $20-40 | $20-40 | $20-40 |
| **Testing/Validation** | $200-500 | $500-1,000 | $1,000-2,000 | $1,500-3,000 |
| **Documentation/Training** | $100-300 | $300-600 | $500-1,000 | $500-1,000 |
| **Total Development Cost** | $800-1,800 | $2,820-5,640 | $4,520-9,040 | $7,020-14,040 |
| **Time to Market** | 2-4 weeks | 6-12 weeks | 8-16 weeks | 12-24 weeks |
| **Maintenance Complexity** | Low | Moderate | Moderate | High |

### Suitability for Pool Applications

| Aspect | Option A | Option B | Option C | Option D |
|--------|----------|----------|----------|----------|
| **Large Pool Systems** | Good | Excellent | Good | Excellent |
| **Small Pool/Spa** | Excellent | Good | Good | Good |
| **Variable Bather Loads** | Good | Excellent | Excellent | Excellent |
| **Weather Disturbances** | Good | Good | Excellent | Excellent |
| **Chemical Cost Savings** | Moderate | High | Moderate | High |
| **Operator Training Required** | Minimal | Moderate | Low | High |

---

## Decision Matrix Analysis

### Scoring Criteria (1-10 scale, 10 = best)

| Criterion | Weight | Option A | Option B | Option C | Option D |
|-----------|--------|----------|----------|----------|----------|
| **Control Performance** | 25% | 7 (good accuracy) | 9 (excellent accuracy) | 8 (good robust) | 9 (adaptive excellent) |
| **Development Cost** | 20% | 10 (lowest cost) | 7 (moderate cost) | 5 (higher cost) | 3 (highest cost) |
| **Implementation Speed** | 20% | 10 (fastest) | 7 (moderate) | 5 (slower) | 3 (slowest) |
| **Chemical Efficiency** | 15% | 7 (good) | 9 (excellent) | 7 (good) | 9 (excellent) |
| **Robustness** | 10% | 9 (very robust) | 6 (stability concerns) | 9 (very robust) | 8 (complex robust) |
| **Tuning Simplicity** | 10% | 9 (very simple) | 4 (complex tuning) | 7 (moderate) | 3 (very complex) |

### Weighted Scores

| Option | Calculation | **Total Score** |
|--------|-------------|-----------------|
| **Option A** | (7×0.25)+(10×0.20)+(10×0.20)+(7×0.15)+(9×0.10)+(9×0.10) | **8.55** |
| **Option B** | (9×0.25)+(7×0.20)+(7×0.20)+(9×0.15)+(6×0.10)+(4×0.10) | **7.60** |
| **Option C** | (8×0.25)+(5×0.20)+(5×0.20)+(7×0.15)+(9×0.10)+(7×0.10) | **6.65** |
| **Option D** | (9×0.25)+(3×0.20)+(3×0.20)+(9×0.15)+(8×0.10)+(3×0.10) | **6.40** |

---

## Final Decision

### Selected Option: Option A - Adaptive Bang-Bang Control ✅ **RECOMMENDED**

**Decision Score:** 8.55/10 (highest rated option balancing performance and implementation)

**Final Configuration:**
- **Core Algorithm:** Enhanced bang-bang control with adaptive deadband
- **Deadband Range:** ±0.05 to ±0.15 pH based on system response
- **Safety Integration:** Hard limits at pH 6.5 and 8.5 with emergency stop
- **Pump Protection:** Minimum runtime (30s) and off-time (2 minutes) enforcement
- **Trend Analysis:** pH rate monitoring for improved response prediction
- **Total Development Cost:** $800-1,800

**Key Decision Factors:**
1. **Rapid Implementation:** Can be delivered in 2-4 weeks vs. months for alternatives
2. **Cost Effectiveness:** Lowest development cost enables faster market entry
3. **Robustness:** Very stable operation, difficult to destabilize
4. **Simplicity:** Easy for installers and operators to understand and tune
5. **ESP32 Compatibility:** Minimal computational requirements

**Implementation Timeline:**
- **Week 1:** Basic bang-bang algorithm implementation and testing
- **Week 2:** Adaptive deadband and trend analysis integration
- **Week 3:** Safety system integration and pump protection
- **Week 4:** Field testing and algorithm refinement

---

## Recommendation

### Primary Recommendation: Option A - Adaptive Bang-Bang Control ✅ **SELECTED**

**Justification for Pool/Spa Applications:**

1. **Appropriate Performance:** ±0.1 pH control adequate for pool/spa applications (not laboratory precision required)
2. **System Compatibility:** Works excellently with on/off peristaltic pumps (no variable speed needed)
3. **Operator Friendly:** Simple deadband adjustment easily understood by pool technicians
4. **Robust Operation:** Proven stability in varying pool conditions and chemical environments
5. **Cost Effective:** Minimal development cost allows focus on other system features

**Implementation Strategy:**

**Phase 1: Core Algorithm (1 week)**
- Implement basic bang-bang control with fixed deadband
- Add pump runtime and off-time protection
- Integrate with pH sensor and chemical pumps
- Test basic functionality and safety integration

**Phase 2: Adaptive Enhancement (1 week)**
- Add pH trend analysis using moving average and derivative
- Implement adaptive deadband based on error magnitude and trend
- Add progressive control (tighter deadband closer to setpoint)
- Test adaptation performance under various conditions

**Phase 3: Optimization and Safety (1 week)**
- Integrate with safety system limits and emergency stops
- Add chemical efficiency monitoring and reporting
- Implement user-configurable parameters (setpoint, deadband limits)
- Create diagnostic and troubleshooting features

### Future Enhancement Path

**Enhanced Algorithm Package (Future Product Option):**
For premium installations requiring tighter control, a PID upgrade package can be offered:

**PID Enhancement Benefits:**
- Improved accuracy: ±0.02-0.05 pH vs. ±0.1 pH
- Better chemical efficiency through continuous modulation
- Enhanced disturbance rejection for high-usage pools
- Professional control for commercial installations

**Upgrade Implementation:**
- Software-only upgrade through IoT connectivity
- Additional PWM control hardware ($30-50 upgrade kit)
- Professional tuning service option
- Market segmentation: basic vs. premium control

### Control Algorithm Implementation

**ESP32 Implementation Example:**
```cpp
class PoolController {
private:
    float setpoint = 7.4;
    float deadband_base = 0.1;
    float deadband_current = 0.1;
    float ph_history[5] = {7.4, 7.4, 7.4, 7.4, 7.4};
    int history_index = 0;
    
    unsigned long pump_start_time = 0;
    unsigned long last_pump_stop = 0;
    const unsigned long MIN_PUMP_TIME = 30000;  // 30 seconds
    const unsigned long MIN_OFF_TIME = 120000;  // 2 minutes
    
public:
    void updateControl(float current_ph) {
        // Update pH history
        ph_history[history_index] = current_ph;
        history_index = (history_index + 1) % 5;
        
        // Calculate pH trend (rate of change)
        float ph_trend = calculateTrend();
        
        // Adapt deadband based on error and trend
        float ph_error = abs(current_ph - setpoint);
        adaptDeadband(ph_error, ph_trend);
        
        // Control logic with safety checks
        if (current_ph > setpoint + deadband_current) {
            if (canStartAcidPump()) {
                startAcidPump();
            }
        } else if (current_ph < setpoint - deadband_current) {
            if (canStartBasePump()) {
                startBasePump();
            }
        } else {
            stopAllPumps();
        }
    }
    
private:
    float calculateTrend() {
        // Simple linear regression on recent pH history
        float sum_x = 0, sum_y = 0, sum_xy = 0, sum_x2 = 0;
        for (int i = 0; i < 5; i++) {
            sum_x += i;
            sum_y += ph_history[i];
            sum_xy += i * ph_history[i];
            sum_x2 += i * i;
        }
        return (5 * sum_xy - sum_x * sum_y) / (5 * sum_x2 - sum_x * sum_x);
    }
    
    void adaptDeadband(float error, float trend) {
        // Increase deadband if trending toward setpoint
        // Decrease deadband if error is small and stable
        if (abs(trend) > 0.01 && ((error > 0 && trend < 0) || (error < 0 && trend > 0))) {
            deadband_current = min(deadband_current * 1.1, 0.15);
        } else if (error < 0.05 && abs(trend) < 0.005) {
            deadband_current = max(deadband_current * 0.95, 0.05);
        }
    }
};
```

This implementation provides reliable, cost-effective pH control suitable for the target pool/spa market while maintaining clear upgrade paths for premium applications requiring tighter control performance.