# pH Balancer Outstanding Design Decisions Document

**Document Number:** PH-BAL-DD-002  
**Project:** Industrial IoT pH Balancer  
**Company:** Cannasol Technologies  
**Author:** Development Team  
**Date:** June 28, 2025  
**Version:** 1.0  
**Status:** Active - Decisions Pending  

---

## Executive Summary

This document identifies all outstanding design decisions required for the Industrial IoT pH Balancer project. Each decision includes multiple viable options with preliminary analysis to guide the decision-making process. These decisions must be resolved to proceed with detailed design and prototyping phases.

**Status:** 17 major design decisions identified, 1 resolved (DD-001)  
**Priority:** Complete all high-priority decisions within 2 weeks  
**Dependencies:** Several decisions are interdependent and must be coordinated  

---

## Table of Contents

1. [Decision Overview Matrix](#1-decision-overview-matrix)
2. [Sensor Systems](#2-sensor-systems)
3. [Chemical Addition Systems](#3-chemical-addition-systems)
4. [Power and Electrical](#4-power-and-electrical)
5. [Mechanical and Environmental](#5-mechanical-and-environmental)
6. [Communication and Interface](#6-communication-and-interface)
7. [Safety and Control](#7-safety-and-control)
8. [Manufacturing and Production](#8-manufacturing-and-production)
9. [Decision Dependencies](#9-decision-dependencies)
10. [Next Steps](#10-next-steps)

---

## 1. Decision Overview Matrix

| Decision ID | Decision Title                  | Priority | Complexity | Est. Impact | Target Date |
|-------------|---------------------------------|----------|------------|-------------|-------------|
| **DD-001**  | pH Sensor Technology            | ✅ RESOLVED | Medium     | High        | COMPLETE    |
| **DD-002**  | Chemical Addition Method        | High     | High       | High        | Week 1      |
| **DD-003**  | Temperature Sensor Selection    | Medium   | Low        | Medium      | Week 1      |
| **DD-004**  | Power Supply Architecture       | High     | Medium     | Medium      | Week 1      |
| **DD-005**  | Enclosure and Mounting          | Medium   | Medium     | Medium      | Week 2      |
| **DD-006**  | Local User Interface            | Low      | Low        | Low         | Week 3      |
| **DD-007**  | Communication Backup Protocol   | Medium   | Medium     | Medium      | Week 2      |
| **DD-008**  | Safety System Architecture      | High     | High       | High        | Week 1      |
| **DD-009**  | Calibration System Design       | Medium   | Medium     | Medium      | Week 2      |
| **DD-010**  | Chemical Storage System         | Medium   | Low        | Medium      | Week 2      |
| **DD-011**  | Flow Measurement Method         | Medium   | Medium     | Medium      | Week 2      |
| **DD-012**  | Data Logging Strategy           | Low      | Low        | Low         | Week 3      |
| **DD-013**  | Control Algorithm Approach      | Medium   | High       | High        | Week 2      |
| **DD-014**  | PCB Design Strategy             | Medium   | Medium     | Medium      | Week 2      |
| **DD-015**  | Connector and Cable Types       | Low      | Low        | Low         | Week 3      |
| **DD-016**  | Antenna and RF Design           | Medium   | Medium     | Medium      | Week 2      |
| **DD-017**  | Environmental Protection Level  | Medium   | Medium     | Medium      | Week 2      |

---

## 2. Sensor Systems

### DD-001: pH Sensor Technology Selection ✅ **RESOLVED**

**Decision Made:** June 28, 2025  
**Selected Option:** Atlas Scientific EZO pH Kit with Flow-Through Cell  
**Final Score:** 8.43/10 (decision matrix analysis)  
**Budget:** $230-340 (within $350 target)  

**Selected Configuration:**
- Atlas Scientific EZO pH Circuit (I2C interface)
- Flow-through pH probe with NPT threading
- Automatic temperature compensation
- Factory calibrated system
- ±0.002 pH accuracy (exceeds ±0.01 requirement)

**Rationale:** With the updated $350 budget, the Atlas Scientific solution provides the best combination of accuracy, reliability, and ease of integration. The factory calibration and proven ESP32 compatibility eliminate development risk while delivering exceptional performance.

**Next Steps:**
- Component procurement (Week 1)
- ESP32 integration testing (Week 2-3)
- Installation validation (Week 4)

**Reference:** See DD-001-pH-sensor.md for complete analysis

---

### DD-003: Temperature Sensor Selection

**Decision Required:** Select temperature sensor for pH compensation and system monitoring

**Options:**

#### Option A: Integrated pH/Temperature Probe

- **Pros:** Single installation point, guaranteed thermal coupling
- **Cons:** Tied to pH sensor selection, higher cost, single point failure
- **Technical:** PT100/PT1000 RTD or NTC thermistor
- **Accuracy:** ±0.1°C typical

#### Option B: Separate Digital Temperature Sensor

**DS18B20 (1-Wire Digital)**

- **Pros:** Simple digital interface, low cost ($2-5), waterproof versions available
- **Cons:** Requires separate installation, potential thermal lag
- **Technical:** -55°C to +125°C, ±0.5°C accuracy, parasitic power option
- **Interface:** 1-Wire protocol, single GPIO pin

**BME280 (I2C Environmental Sensor)**

- **Pros:** Temperature + humidity + pressure, I2C interface, very low cost
- **Cons:** Not waterproof, limited temperature range, humidity not needed
- **Technical:** -40°C to +85°C, ±1°C accuracy, 3.3V operation
- **Interface:** I2C, multiple sensors on same bus

#### Option C: Analog Temperature Sensor

**LM35/TMP36 Linear Temperature Sensor**

- **Pros:** Simple analog interface, very low cost (<$2), wide availability
- **Cons:** Lower accuracy, requires ADC, analog calibration needed
- **Technical:** -40°C to +100°C, ±2°C accuracy, 10mV/°C output
- **Interface:** Direct to ESP32 ADC

**Decision Criteria:**

- Accuracy: ±0.5°C for pH compensation
- Range: 0°C to 50°C minimum operating range
- Response time: <30 seconds for control applications
- Cost: <$10 target
- Reliability: Industrial environment compatible

---

## 3. Chemical Addition Systems

### DD-002: Chemical Addition Method Selection

**Decision Required:** Select method and hardware for acid/base chemical addition

**Options:**

#### Option A: Peristaltic Pumps

**Small Peristaltic Dosing Pumps (12V DC)**

- **Pros:** Self-priming, accurate dosing, chemical compatibility, reversible
- **Cons:** Tube wear, higher cost ($50-100 each), potential tube failure
- **Technical:** 0.1-100 mL/min flow rates, ±2% accuracy, tube life 1-2 years
- **Control:** PWM speed control or on/off with timer
- **Suppliers:** Watson-Marlow, Cole-Parmer, Kamoer

**Mini Peristaltic Pumps (5V DC)**

- **Pros:** Lower cost ($20-40), direct ESP32 control, compact size
- **Cons:** Lower accuracy, shorter tube life, limited chemical compatibility
- **Technical:** 0.1-10 mL/min flow rates, ±5% accuracy, food-grade tubing
- **Control:** Simple on/off relay or PWM
- **Applications:** Lower precision requirements

#### Option B: Solenoid Metering Valves

**Proportional Solenoid Valves**

- **Pros:** No moving parts in fluid, very precise control, long life
- **Cons:** Requires pressurized chemical supply, higher cost, complex control
- **Technical:** 0.01-10 mL pulse volume, ±1% accuracy, 12-24V operation
- **Control:** Pulse width modulation, requires pressure feedback
- **Applications:** High precision, industrial installations

**On/Off Solenoid Valves + Pressure Regulation**

- **Pros:** Simple control, reliable operation, wide chemical compatibility
- **Cons:** Requires pressure system, timing-based dosing only
- **Technical:** Fast open/close (<100ms), pressure dependent flow
- **Control:** Timed pulses, requires flow calibration
- **Applications:** Simple bang-bang control systems

#### Option C: Syringe Pumps

**Stepper Motor Driven Syringes**

- **Pros:** Extremely precise dosing, programmable volumes, no tube wear
- **Cons:** Limited volume, complex mechanics, higher cost
- **Technical:** μL to mL precision, stepper motor control required
- **Control:** Step counting for precise volume delivery
- **Applications:** Laboratory precision requirements

#### Option D: Gravity Feed + Control Valves

**Gravity Fed Chemical Reservoirs**

- **Pros:** No pumps to fail, simple installation, low power
- **Cons:** Requires elevated chemical storage, pressure varies with level
- **Technical:** Height dependent pressure, requires level sensing
- **Control:** Valve timing with level compensation
- **Applications:** Simple installations with adequate height

**Decision Criteria:**

- Accuracy: ±5% flow rate accuracy minimum
- Volume range: 0.1 mL to 100 mL per dose
- Chemical compatibility: Acids, bases, common pool chemicals
- Reliability: 6+ months continuous operation
- Cost: <$150 total for acid + base systems
- Control: Must interface with ESP32 GPIO/PWM

---

## 4. Power and Electrical

### DD-004: Power Supply Architecture Selection

**Decision Required:** Define power distribution and supply strategy for entire system

**Options:**

#### Option A: Centralized 24VDC System

**Single 24VDC Supply with Local Regulation**

- **Pros:** Industrial standard, powers pumps directly, compatible with existing Cannasol infrastructure
- **Cons:** Requires DC-DC conversion for ESP32, higher wiring complexity
- **Technical:** 24VDC main → 12V (pumps) → 5V → 3.3V (ESP32)
- **Components:** Buck converters, isolation if needed
- **Power Budget:** 2-5A @ 24VDC depending on pump count

**24VDC with Isolated Supplies**

- **Pros:** Electrical isolation, noise immunity, safety compliance
- **Cons:** Higher cost, more complex, larger size
- **Technical:** Isolated DC-DC converters for each voltage level
- **Applications:** Industrial safety requirements, noisy environments

#### Option B: Dual Voltage System

**12VDC for Pumps + 5VDC for Electronics**

- **Pros:** Common voltage levels, simpler regulation, lower component stress
- **Cons:** Dual supply complexity, less industrial standard
- **Technical:** 12VDC for pumps, 5VDC → 3.3V for ESP32
- **Components:** Two supplies or 12V → 5V buck converter
- **Wiring:** Separate power feeds or local conversion

#### Option C: USB/Low Voltage System

**5VDC System with Low Power Pumps**

- **Pros:** Simple supply, USB power option, low cost
- **Cons:** Limited to small pumps, may not meet performance requirements
- **Technical:** 5VDC throughout, 3.3V regulation for ESP32 only
- **Applications:** Laboratory/desktop applications, low power requirements

#### Option D: Renewable/Battery Backup

**Solar Panel + Battery System**

- **Pros:** Remote installation capability, backup power, environmentally friendly
- **Cons:** Complex power management, weather dependent, higher cost
- **Technical:** Solar charge controller, LiPo batteries, low power design
- **Applications:** Remote locations, grid-independent operation

**Decision Criteria:**

- Power requirement: 10-50W total system
- Voltage compatibility: ESP32 (3.3V), pumps (5-24V), sensors (3.3-12V)
- Safety: Industrial safety standards compliance
- Cost: <$50 power supply cost target
- Reliability: Continuous operation capability
- Installation: Compatible with existing Cannasol power infrastructure

---

## 5. Mechanical and Environmental

### DD-005: Enclosure and Mounting Selection

**Decision Required:** Select enclosure type, size, and mounting method

**Options:**

#### Option A: NEMA/IP Rated Industrial Enclosures

**NEMA 4X Polycarbonate Enclosure**

- **Pros:** Weather resistant, chemical resistant, clear cover options, industrial standard
- **Cons:** Higher cost ($50-100), larger size, heat dissipation concerns
- **Technical:** IP65/66 rating, UV resistant, mounting flanges
- **Sizes:** 6"×4"×3" to 12"×8"×6" typical options
- **Mounting:** Wall mount, pole mount, panel mount options

**Stainless Steel NEMA Enclosure**

- **Pros:** Ultimate chemical resistance, professional appearance, very durable
- **Cons:** High cost ($100-200), heavier, potential EMI shielding issues
- **Technical:** 316SS typical, IP66/67 ratings available
- **Applications:** Harsh chemical environments, food grade requirements

#### Option B: DIN Rail Enclosures

**Standard DIN Rail Housing**

- **Pros:** Professional industrial appearance, standardized mounting, modular
- **Cons:** Limited size options, requires DIN rail infrastructure
- **Technical:** 35mm DIN rail, modular widths (e.g., 6 modules wide)
- **Integration:** Matches existing Cannasol automation hardware

**Extended DIN Rail Enclosure**

- **Pros:** More space for components, maintains DIN rail compatibility
- **Cons:** Custom solutions may be needed, higher cost
- **Technical:** Multiple module widths, integrated mounting

#### Option C: Custom/3D Printed Enclosures

**3D Printed Prototype Enclosures**

- **Pros:** Perfect fit for components, rapid iteration, low prototype cost
- **Cons:** Limited material options, environmental concerns, production scaling
- **Technical:** ABS/PETG materials, IP rating difficult to achieve
- **Applications:** Prototyping phase, low-volume custom applications

**Injection Molded Custom Enclosure**

- **Pros:** Perfect design optimization, cost effective at volume, branding options
- **Cons:** High tooling cost ($10,000+), long lead times, minimum quantities
- **Technical:** Multiple material options, integrated features possible
- **Applications:** High volume production (1000+ units)

**Decision Criteria:**

- Environmental protection: IP65 minimum for outdoor use
- Size: Accommodate PCB (100×80mm est.) + connectors + wiring space
- Chemical resistance: Compatible with pool/spa chemicals
- Temperature range: -10°C to +60°C operating
- Cost: <$75 in low volumes, <$25 at production volumes
- Mounting: Wall mount and pole mount options required

---

### DD-017: Environmental Protection Level

**Decision Required:** Define required environmental protection specifications

**Options:**

#### Option A: Indoor/Protected Installation (IP54)

- **Pros:** Lower cost enclosures, simpler sealing, more connector options
- **Cons:** Requires protective installation location, limited outdoor use
- **Technical:** Dust protection, splash resistant, not submersible
- **Applications:** Indoor pump rooms, covered outdoor installations

#### Option B: Outdoor Weather Resistant (IP65)

- **Pros:** Direct outdoor installation, rain resistant, moderate cost increase
- **Cons:** Not submersible, limited to normal environmental conditions
- **Technical:** Dust tight, low pressure water jets from any direction
- **Applications:** Standard outdoor pool/spa installations

#### Option C: Harsh Environment (IP67)

- **Pros:** Temporary submersion resistant, very robust, wider application range
- **Cons:** Higher enclosure cost, more complex sealing requirements
- **Technical:** Dust tight, immersion up to 1m for 30 minutes
- **Applications:** Flood-prone areas, wash-down environments

**Decision Criteria:**

- Installation environment: Indoor protected to outdoor exposed
- Water exposure: Splash to temporary submersion
- Chemical exposure: Standard pool chemicals
- Cost impact: Balance protection level with market requirements

---

## 6. Communication and Interface

### DD-006: Local User Interface Selection

**Decision Required:** Select local display and control interface options

**Options:**

#### Option A: No Local Interface (Mobile/Web Only)

- **Pros:** Lowest cost, simplest design, reduces failure points
- **Cons:** No local diagnostics, requires mobile device for all interaction
- **Technical:** Status LEDs only for basic indication
- **Applications:** Simple installations with reliable WiFi

#### Option B: Basic LED Status Display

**Multi-Color Status LEDs**

- **Pros:** Low cost ($5-10), simple interface, always visible
- **Cons:** Limited information display, requires documentation to interpret
- **Technical:** 3-5 LEDs for power, WiFi, pH status, pump operation
- **Information:** Status only, no numeric values

#### Option C: LCD/OLED Display

**16×2 Character LCD**

- **Pros:** Numeric pH display, error messages, moderate cost ($10-15)
- **Cons:** Limited information, requires backlight, temperature sensitive
- **Technical:** I2C interface, 5V or 3.3V operation
- **Information:** Current pH, setpoint, basic status

**128×64 OLED Display**

- **Pros:** High quality display, graphics capability, low power
- **Cons:** Higher cost ($15-25), limited size, programming complexity
- **Technical:** I2C/SPI interface, white or color options
- **Information:** pH trends, detailed status, configuration menus

#### Option D: Touchscreen Interface

**Small Resistive Touchscreen**

- **Pros:** Complete local control, professional appearance, configuration capability
- **Cons:** High cost ($30-50), complex programming, environmental concerns
- **Technical:** 2.4"-4" typical, SPI interface, requires graphics library
- **Applications:** Premium installations, complex configuration needs

**Decision Criteria:**

- Information needs: Status indication vs. numeric values vs. full configuration
- Cost impact: Target <$20 additional cost
- Environmental: Must work in humid, chemical environment
- User experience: Balance simplicity with functionality

---

### DD-007: Communication Backup Protocol

**Decision Required:** Select backup communication method if ESP-NOW/WiFi fails

**Options:**

#### Option A: No Backup (ESP-NOW/WiFi Only)

- **Pros:** Simplest design, lowest cost, minimal complexity
- **Cons:** Single point of failure, no communication during network issues
- **Technical:** ESP-NOW primary, WiFi secondary for configuration
- **Risk Assessment:** Acceptable for non-critical applications

#### Option B: Bluetooth Backup

**Bluetooth Classic or BLE**

- **Pros:** Built into ESP32, no additional hardware, moderate range
- **Cons:** Limited range (10m), pairing complexity, mobile app required
- **Technical:** ESP32 built-in Bluetooth, custom mobile app needed
- **Applications:** Local troubleshooting and configuration

#### Option C: RS485/MODBUS RTU

**Wired MODBUS RTU Connection**

- **Pros:** Industrial standard, long range (1200m), noise immune, integrates with existing systems
- **Cons:** Requires wiring installation, additional hardware cost ($10-15)
- **Technical:** RS485 transceiver, MODBUS RTU protocol
- **Applications:** Integration with existing industrial automation

#### Option D: LoRaWAN Long Range

**LoRa Radio Module**

- **Pros:** Very long range (10+ km), low power, no infrastructure required
- **Cons:** Low data rate, additional hardware cost ($20-30), licensing complexity
- **Technical:** 915MHz ISM band, low power wide area network
- **Applications:** Remote installations, campus-wide networks

**Decision Criteria:**

- Range requirements: 10m to 1km+ depending on application
- Data rate needs: pH data is low bandwidth
- Infrastructure: Existing networks vs. standalone
- Cost: Additional hardware and development costs
- Reliability: Backup system reliability vs. primary system

---

## 7. Safety and Control

### DD-008: Safety System Architecture

**Decision Required:** Define safety system scope, sensors, and fail-safe behaviors

**Options:**

#### Option A: Basic Software Safety

**Software-Only Safety Logic**

- **Pros:** No additional hardware cost, flexible logic, easy updates
- **Cons:** Software failure modes, no hardware backup, regulatory limitations
- **Technical:** Software timeouts, range checking, pump interlock logic
- **Components:** ESP32 software only
- **Applications:** Non-critical applications, residential use

#### Option B: Hardware Safety Interlocks

**Dedicated Safety Input Circuits**

- **Pros:** Hardware independence, fail-safe operation, regulatory compliance
- **Cons:** Additional hardware cost ($20-40), more complex design
- **Technical:** Hardware emergency stop, pump enable relays, watchdog circuits
- **Components:** Safety relays, emergency stop buttons, flow switches

**Emergency Stop System**

- **Pros:** Immediate chemical dosing stop, user safety, regulatory compliance
- **Cons:** Requires user training, potential false alarms, reset procedures
- **Technical:** Normally closed contact, pump power interruption
- **Installation:** Accessible emergency stop button, clear labeling

#### Option C: Advanced Monitoring

**Chemical Level and Flow Monitoring**

- **Pros:** Prevents run-dry conditions, chemical inventory tracking, predictive maintenance
- **Cons:** Additional sensors and cost ($30-60), complexity increase
- **Technical:** Level switches, flow meters, chemical consumption tracking
- **Benefits:** Prevents damage, alerts for chemical refill

**pH Range Safety Limits**

- **Pros:** Prevents dangerous pH excursions, protects equipment and users
- **Cons:** May cause false alarms, requires careful tuning
- **Technical:** Software limits with hardware backup, alarm outputs
- **Implementation:** Configurable high/low pH limits with increasing alarm severity

**Decision Criteria:**

- Safety requirements: Residential vs. commercial vs. industrial
- Regulatory compliance: Local codes and standards
- Cost impact: Safety system cost vs. total system cost
- Reliability: Hardware vs. software safety approaches
- User training: Complexity of safety system operation

---

### DD-013: Control Algorithm Approach

**Decision Required:** Select pH control algorithm and implementation strategy

**Options:**

#### Option A: Simple Bang-Bang Control

**On/Off Control with Deadband**

- **Pros:** Simple implementation, easy tuning, robust operation
- **Cons:** Oscillation around setpoint, chemical waste, wear on pumps
- **Technical:** Turn on pump when pH > setpoint + deadband, off when < setpoint - deadband
- **Parameters:** Deadband (±0.1 pH typical), minimum on/off times
- **Applications:** Simple systems, large buffer volumes

#### Option B: PID Control

**Proportional-Integral-Derivative Control**

- **Pros:** Smooth control, minimal overshoot, industry standard
- **Cons:** Tuning complexity, potential instability, requires pump speed control
- **Technical:** Classic PID algorithm with gain scheduling
- **Parameters:** Kp, Ki, Kd gains, integral windup protection
- **Implementation:** Requires PWM pump control or pulse width modulation

**Adaptive PID Control**

- **Pros:** Self-tuning capability, optimal performance over time
- **Cons:** Complex implementation, longer settling time during adaptation
- **Technical:** Parameter estimation, gain adaptation algorithms
- **Applications:** Systems with varying characteristics

#### Option C: Model Predictive Control (MPC)

**Advanced Predictive Algorithm**

- **Pros:** Excellent performance, constraint handling, predictive capability
- **Cons:** High computational requirements, complex tuning, implementation difficulty
- **Technical:** System model, optimization solver, prediction horizon
- **Applications:** High-performance requirements, research applications

#### Option D: Fuzzy Logic Control

**Rule-Based Fuzzy Controller**

- **Pros:** Intuitive tuning, robust to parameter variations, handles nonlinearities well
- **Cons:** Rule development complexity, computational overhead
- **Technical:** Membership functions, rule base, defuzzification
- **Applications:** Systems with expert knowledge, nonlinear behavior

**Decision Criteria:**

- Performance requirements: Settling time, overshoot, steady-state accuracy
- Tuning complexity: Automatic vs. manual tuning requirements
- Computational resources: ESP32 processing limitations
- Chemical efficiency: Minimize chemical waste
- Robustness: Performance under varying conditions

---

## 8. Manufacturing and Production

### DD-014: PCB Design Strategy

**Decision Required:** Select PCB design approach and manufacturing strategy

**Options:**

#### Option A: Single-Board Design

**All Components on One PCB**

- **Pros:** Lowest cost, simplest assembly, minimal interconnections
- **Cons:** Larger board size, mixed analog/digital design challenges
- **Technical:** 4-layer PCB recommended, careful analog/digital separation
- **Size:** 100×80mm estimated for all functions
- **Applications:** Volume production, cost optimization

#### Option B: Modular PCB Design

**Separate Boards for Different Functions**

- **Pros:** Design optimization per function, easier testing, parallel development
- **Cons:** Higher cost, more interconnections, larger total volume
- **Technical:** Main controller + sensor interface + power board
- **Advantages:** Analog isolation, independent optimization, easier troubleshooting

#### Option C: Development Board + Shield

**ESP32 Dev Board with Custom Shield**

- **Pros:** Fastest development, proven ESP32 board, easy prototyping
- **Cons:** Higher cost, larger size, less professional appearance
- **Technical:** Standard ESP32 dev board + custom interface shield
- **Applications:** Prototyping, low volume production, development

#### Option D: System-on-Module (SoM) Approach

**ESP32 Module + Custom Carrier Board**

- **Pros:** Certified wireless module, reduced RF design complexity, FCC compliance
- **Cons:** Higher per-unit cost, limited customization, dependency on module availability
- **Technical:** Pre-certified ESP32 module + custom interface circuitry
- **Applications:** Simplified regulatory approval, faster time-to-market

**Decision Criteria:**

- Development speed: Time to prototype and first production
- Cost optimization: Unit cost at different volume levels
- Design complexity: RF design, analog circuits, power management
- Manufacturing: Assembly complexity, testing requirements
- Regulatory: FCC/CE certification requirements

---

### DD-015: Connector and Cable Types

**Decision Required:** Select connectors for sensors, pumps, power, and communication

**Options:**

#### Option A: Industrial Screw Terminals

**Phoenix Contact or Weidmuller Style**

- **Pros:** Field wiring friendly, secure connections, wire size flexibility
- **Cons:** Larger PCB footprint, more expensive, assembly time
- **Technical:** 12-24 AWG wire capacity, 300V ratings available
- **Applications:** Permanent installations, industrial environments

#### Option B: Sealed Circular Connectors

**M12 or M8 Circular Connectors**

- **Pros:** IP67 sealing, professional appearance, secure connection
- **Cons:** Higher cost, pre-made cable assemblies required
- **Technical:** 3-8 pin configurations, A-coded or sensor-specific
- **Applications:** Harsh environments, frequent connection/disconnection

#### Option C: Standard Board Connectors

**JST, Molex, or Similar Board-to-Wire**

- **Pros:** Low cost, compact size, variety of configurations
- **Cons:** Limited environmental sealing, smaller wire gauge
- **Technical:** 22-28 AWG typical, various pitch options
- **Applications:** Internal connections, protected environments

#### Option D: Mixed Connector Strategy

**Different Connectors for Different Functions**

- **Pros:** Optimized for each application, cost/performance balance
- **Cons:** Multiple connector types to stock, assembly complexity
- **Technical:** Power (screw terminals), sensors (M12), data (RJ45)
- **Applications:** Professional installations with varied requirements

**Decision Criteria:**

- Environmental sealing: IP rating requirements per application
- Wire gauge requirements: Power vs. signal vs. sensor cables
- Installation environment: Field wiring vs. factory assembly
- Cost impact: Connector cost vs. cable assembly cost
- Reliability: Connection integrity over time

---

### DD-016: Antenna and RF Design

**Decision Required:** Select antenna type and RF implementation approach

**Options:**

#### Option A: External Antenna with U.FL Connector

**High-Gain Directional Antenna**

- **Pros:** Maximum range, professional installation, optimal placement
- **Cons:** Installation complexity, additional cost, mechanical considerations
- **Technical:** 5-9 dBi gain typical, 50Ω impedance, weatherproof
- **Range:** 200-500m depending on installation and obstacles

**Omnidirectional External Antenna**

- **Pros:** 360° coverage, easier installation than directional
- **Cons:** Lower gain than directional, still requires mounting
- **Technical:** 2-5 dBi gain typical, compact form factor
- **Applications:** Central location installations

#### Option B: PCB Trace Antenna

**Integrated PCB Antenna Design**

- **Pros:** Lowest cost, no external components, compact design
- **Cons:** Limited range, sensitive to enclosure effects, design complexity
- **Technical:** Quarter-wave or meander design, ground plane requirements
- **Range:** 50-150m typical, highly dependent on implementation

#### Option C: Chip Antenna

**Surface Mount Ceramic Antenna**

- **Pros:** Small size, consistent performance, no external antenna needed
- **Cons:** Limited range, cost higher than PCB trace, ground plane sensitive
- **Technical:** Standard components available, various gain options
- **Range:** 100-200m typical with proper ground plane design

#### Option D: Flexible/Whip Antenna

**External Flexible Antenna**

- **Pros:** Good performance, adjustable positioning, moderate cost
- **Cons:** Mechanical vulnerability, less professional appearance
- **Technical:** Quarter-wave rubber duck style, SMA/RP-SMA connectors
- **Applications:** Portable or temporary installations

**Decision Criteria:**

- Range requirements: Indoor (50m) vs. outdoor (200m+) applications
- Installation constraints: Enclosure size, mounting options
- Cost impact: Antenna cost vs. performance requirements
- Mechanical requirements: Vibration, impact resistance
- Regulatory: FCC/CE compliance with chosen antenna

---

## 9. Decision Dependencies

### 9.1 Critical Decision Paths

**Path 1: Sensor → Power → Control Algorithm**

```
DD-001 (pH Sensor) → DD-003 (Temperature) → DD-004 (Power) → DD-013 (Control Algorithm)
```

- pH sensor selection drives power requirements and control capabilities
- Digital sensors simplify control but may require different power architecture
- Temperature sensor affects pH compensation algorithm complexity

**Path 2: Chemical Addition → Safety → Enclosure**

```
DD-002 (Chemical Addition) → DD-008 (Safety) → DD-005 (Enclosure) → DD-017 (Protection Level)
```

- Pump type determines safety requirements and enclosure size
- Safety system complexity affects enclosure space and environmental needs
- High-pressure systems require different protection levels

**Path 3: Communication → Interface → Manufacturing**

```markdown
DD-007 (Backup Comm) → DD-006 (User Interface) → DD-014 (PCB Design) → DD-015 (Connectors)
```

- Backup communication affects PCB complexity and connector requirements
- Local interface requirements drive PCB size and power consumption
- Manufacturing approach influences connector and cable strategies

### 9.2 Independent Decisions

**Can be decided in parallel:**

- DD-009 (Calibration System)
- DD-010 (Chemical Storage)
- DD-011 (Flow Measurement)
- DD-012 (Data Logging)
- DD-016 (Antenna/RF)

### 9.3 Decision Impact Matrix

| Decision                   | Affects Cost | Affects Schedule | Affects Performance | Affects Reliability |
|----------------------------|--------------|------------------|---------------------|---------------------|
| DD-001 (pH Sensor)         | ★★★          | ★★               | ★★★                 | ★★★                 |
| DD-002 (Chemical Addition) | ★★★          | ★★               | ★★★                 | ★★                  |
| DD-004 (Power Supply)      | ★★           | ★                | ★                   | ★★                  |
| DD-008 (Safety System)     | ★★           | ★★               | ★                   | ★★★                 |
| DD-013 (Control Algorithm) | ★            | ★★★              | ★★★                 | ★★                  |
| DD-014 (PCB Design)        | ★★           | ★★★              | ★                   | ★★                  |

*★ = Low Impact, ★★ = Medium Impact, ★★★ = High Impact*

---

## 10. Next Steps

### 10.1 Immediate Actions (Week 1)

**High Priority Decisions Required:**

1. **DD-001 (pH Sensor)** - Order evaluation samples for testing
2. **DD-002 (Chemical Addition)** - Define pump requirements and test setup
3. **DD-004 (Power Supply)** - Finalize system power architecture
4. **DD-008 (Safety System)** - Define safety requirements and regulatory needs

**Research Tasks:**

- pH sensor accuracy testing with pool/spa chemicals
- Pump flow rate and accuracy characterization
- Power consumption measurements of ESP32 + peripherals
- Safety standard research for chemical dosing systems

### 10.2 Decision Process

**Decision Authority:**

- Technical decisions: Engineering team lead
- Cost decisions: Project manager + engineering
- Safety decisions: Engineering + regulatory compliance
- Manufacturing decisions: Engineering + production team

**Decision Documentation:**

- Each decision will be documented in separate decision record
- Trade-off analysis required for all high-impact decisions
- Prototype testing required for critical component selections
- Vendor evaluations for key components

### 10.3 Risk Mitigation

**Decision Delays:**

- Parallel prototyping of leading options where possible
- Early supplier engagement for long-lead-time components
- Fallback options identified for critical decisions

**Integration Risks:**

- Early breadboard integration testing
- System-level testing plan before PCB design
- Component compatibility verification matrix

---

## Document Control

**Review Schedule:**

- Weekly decision review meetings
- Decision status updates in project reports
- Complete document update after each decision milestone

**Distribution:**

- Development team (all decisions)
- Project management (status and schedule impacts)
- Executive team (cost and business impact decisions)

---

**Document Approval:**

| Role | Name | Date | Status |
|------|------|------|--------|
| **Project Manager** | [Name] | | Pending |
| **Engineering Lead** | [Name] | | Pending |
| **Product Manager** | [Name] | | Pending |

---

*This document contains proprietary and confidential information of Cannasol Technologies. Distribution is restricted to authorized personnel only.*
