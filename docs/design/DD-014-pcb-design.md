# DD-014: PCB Design Strategy

## Detailed Design Decision Analysis

**Document Number:** PH-BAL-DD-014-DETAIL  
**Project:** Industrial IoT pH Balancer  
**Company:** Cannasol Technologies  
**Author:** Development Team  
**Date:** June 28, 2025  
**Version:** 1.0  
**Status:** Decision Pending  

---

## Decision Statement

**Select PCB design approach and manufacturing strategy for the Industrial IoT pH Balancer electronics system.**

**Requirements:**

- **Size Constraints:** Fit within selected enclosure (approximately 100×80mm)
- **Component Integration:** ESP32, power supplies, relays, sensors, connectors
- **Manufacturing:** Support prototype through production volumes
- **Cost Target:** <$100 PCB cost in low volumes, <$50 at production volumes
- **Reliability:** Industrial-grade reliability for outdoor installations
- **Assembly:** Support both hand assembly and automated production
- **Testing:** Built-in test points and debugging capabilities
- **Expandability:** Provisions for future feature additions
- **EMC Compliance:** Meet electromagnetic compatibility requirements
- **Safety:** Proper isolation between low voltage and mains circuits

---

## Option Analysis

### Option A: Single-Board Design

#### Technical Specifications

**Integrated Single PCB Approach**

**Core Components:**
- ESP32-WROOM-UE module with antenna
- Power supply circuits (AC/DC conversion, battery backup)
- Relay drivers for pump control
- Sensor interfaces (pH, temperature, flow)
- Communication interfaces and status LEDs
- All connectors and terminal blocks

**Performance Specifications:**
- **Board Size:** 100×80mm maximum
- **Layer Count:** 4-layer PCB for signal integrity
- **Component Density:** High density layout required
- **Power Sections:** Proper isolation between mains and low voltage
- **Signal Routing:** Careful routing for analog and RF signals
- **Test Points:** Comprehensive test point coverage

**PCB Layout Considerations:**
```
Single Board Layout:
┌─────────────────────────────────┐
│ AC Input   │ ESP32 Module       │
│ & Power    │ & RF Section       │
├─────────────────────────────────┤
│ Relay      │ Sensor Interface   │
│ Drivers    │ & Conditioning     │
├─────────────────────────────────┤
│ Terminal Blocks & Connectors    │
└─────────────────────────────────┘
```

#### Cost Analysis
- **PCB Fabrication (4-layer):** $15-40 per board (low volume)
- **PCB Fabrication (4-layer):** $5-15 per board (high volume)
- **Assembly Cost:** $25-50 (low volume), $10-25 (high volume)
- **Design/Layout Cost:** $5,000-10,000 (one-time)
- **Total Unit Cost:** $40-90 (low), $15-40 (high volume)

#### Advantages
✅ **Compact Size:** Minimal overall system footprint  
✅ **Lower Assembly Cost:** Single board assembly process  
✅ **Better EMC:** Shorter interconnects reduce EMI  
✅ **Cost Effective at Volume:** Lowest per-unit cost in production  
✅ **Reliable Connections:** No inter-board connectors to fail  

#### Disadvantages
❌ **High Component Density:** Difficult layout and thermal management  
❌ **Single Point Failure:** Entire board replacement if any section fails  
❌ **Design Complexity:** Complex layout with mixed signal challenges  
❌ **Testing Difficulty:** Hard to isolate and test individual sections  

---

### Option B: Modular PCB Design

#### Technical Specifications

**Multi-Board Modular System**

**Core Components:**
- Main control board with ESP32 and basic I/O
- Power supply board with AC/DC conversion and battery backup
- I/O expansion board with relays and sensor interfaces
- Standard inter-board connectors
- Modular assembly system

**Performance Specifications:**
- **Board Count:** 2-3 separate PCBs
- **Board Size:** 60×50mm typical per board
- **Layer Count:** 2-layer PCBs sufficient for most boards
- **Interconnects:** Standard pin headers or ribbon cables
- **Modularity:** Individual boards can be tested and replaced

**Modular Architecture:**
```
Modular Design:
┌─────────────┐  ┌─────────────┐
│ Power       │  │ Main Control│
│ Supply      │  │ ESP32       │
│ Board       │  │ Board       │
└─────────────┘  └─────────────┘
       │                │
       └────────┬───────┘
                │
┌─────────────────────────┐
│ I/O Expansion Board     │
│ Relays, Sensors, LEDs   │
└─────────────────────────┘
```

#### Cost Analysis
- **PCB Fabrication (3×2-layer):** $10-25 per set (low volume)
- **PCB Fabrication (3×2-layer):** $3-8 per set (high volume)
- **Assembly Cost:** $30-60 (low volume), $15-35 (high volume)
- **Interconnect Hardware:** $5-15 per set
- **Design Cost:** $3,000-8,000 (distributed across boards)
- **Total Unit Cost:** $45-100 (low), $23-58 (high volume)

#### Advantages
✅ **Design Simplicity:** Each board has focused functionality  
✅ **Easy Testing:** Individual boards can be tested separately  
✅ **Serviceability:** Replace only failed board section  
✅ **Parallel Development:** Different teams can work on different boards  
✅ **Flexible Configuration:** Different I/O boards for different applications  

#### Disadvantages
❌ **Higher System Cost:** Multiple PCBs and connectors increase cost  
❌ **Reliability Risk:** Inter-board connectors can fail  
❌ **Larger Size:** Multiple boards require more enclosure space  
❌ **EMC Challenges:** Longer interconnects can cause EMI issues  

---

### Option C: Development Board + Shield Approach

#### Technical Specifications

**Commercial Dev Board with Custom Shield**

**Core Components:**
- Commercial ESP32 development board (ESP32-DevKitC or similar)
- Custom shield PCB with application-specific circuits
- Stackable header system for easy assembly
- Breakout connectors for all I/O functions
- Minimal custom PCB design required

**Performance Specifications:**
- **Dev Board:** Commercial ESP32 board ($10-20)
- **Shield Size:** 80×60mm custom PCB
- **Layer Count:** 2-layer shield PCB
- **Assembly:** Simple through-hole mounting
- **Expandability:** Easy to add additional shields

#### Cost Analysis
- **ESP32 Dev Board:** $10-20
- **Custom Shield PCB:** $5-15 (2-layer)
- **Shield Assembly:** $10-25
- **Connectors/Headers:** $5-15
- **Design Cost:** $1,000-3,000 (simple shield)
- **Total Unit Cost:** $30-75

#### Advantages
✅ **Rapid Development:** Leverage existing dev board design  
✅ **Low Design Cost:** Minimal custom PCB design required  
✅ **Easy Prototyping:** Breadboard-friendly for development  
✅ **Quick Time-to-Market:** Fastest development path  
✅ **Proven Hardware:** Commercial dev board already tested  

#### Disadvantages
❌ **Non-Professional Appearance:** Dev board aesthetics not commercial  
❌ **Size Inefficiency:** Larger than optimized custom design  
❌ **Cost at Volume:** Higher per-unit cost than custom boards  
❌ **Limited Optimization:** Cannot optimize layout for specific application  

---

### Option D: System-on-Module (SoM) Approach

#### Technical Specifications

**ESP32 SoM with Custom Carrier Board**

**Core Components:**
- Commercial ESP32 System-on-Module
- Custom carrier board with application circuits
- Professional appearance and optimized layout
- Standard module footprint for sourcing flexibility
- Castellated edge connections or LGA package

**Performance Specifications:**
- **SoM Module:** ESP32-S3-WROOM or similar ($8-15)
- **Carrier Board:** 4-layer custom PCB
- **Size:** Optimized for application requirements
- **Assembly:** SMT assembly with SoM placement
- **Flexibility:** Multiple SoM vendors possible

#### Cost Analysis
- **ESP32 SoM:** $8-15
- **Carrier PCB (4-layer):** $10-30
- **Assembly Cost:** $15-35
- **Design Cost:** $4,000-8,000
- **Total Unit Cost:** $33-80 (low), $18-45 (high volume)

#### Advantages
✅ **Professional Design:** Optimized carrier board layout  
✅ **SoM Flexibility:** Multiple module vendors and options  
✅ **Regulatory Benefits:** SoM handles RF certification  
✅ **Size Optimization:** Compact design possible  
✅ **Manufacturing Scale:** Good for mid to high volumes  

#### Disadvantages
❌ **Module Dependency:** Reliant on SoM vendor availability  
❌ **Higher NRE:** Significant carrier board design cost  
❌ **Assembly Complexity:** SMT assembly more complex than through-hole  

---

## Comparative Analysis

### Development and Manufacturing Comparison

| Criterion | Single Board (A) | Modular (B) | Dev Board + Shield (C) | SoM + Carrier (D) |
|-----------|------------------|-------------|----------------------|-------------------|
| **Design Complexity** | High | Moderate | Low | Moderate |
| **Development Time** | 8-12 weeks | 6-10 weeks | 2-4 weeks | 6-8 weeks |
| **NRE Cost** | $5,000-10,000 | $3,000-8,000 | $1,000-3,000 | $4,000-8,000 |
| **Time to Market** | Slow | Moderate | Fast | Moderate |
| **Prototype Cost** | High | Moderate | Low | Moderate |
| **Production Scalability** | Excellent | Good | Poor | Excellent |

### Cost Analysis (Per Unit)

| Volume | Option A | Option B | Option C | Option D |
|--------|----------|----------|----------|----------|
| **10 units** | $80-120 | $90-150 | $40-80 | $70-120 |
| **100 units** | $50-80 | $60-100 | $35-70 | $45-85 |
| **1000 units** | $25-50 | $35-70 | $30-65 | $25-55 |
| **10000 units** | $15-35 | $25-60 | $30-65 | $18-45 |

### Technical Performance Comparison

| Aspect | Option A | Option B | Option C | Option D |
|--------|----------|----------|----------|----------|
| **EMC Performance** | Excellent | Good | Fair | Excellent |
| **Thermal Management** | Challenging | Good | Fair | Good |
| **Serviceability** | Poor | Excellent | Good | Fair |
| **Size Efficiency** | Excellent | Fair | Poor | Good |
| **Signal Integrity** | Excellent | Good | Fair | Excellent |
| **Power Efficiency** | Excellent | Good | Good | Excellent |

---

## Decision Matrix Analysis

### Scoring Criteria (1-10 scale, 10 = best)

| Criterion | Weight | Single Board (A) | Modular (B) | Dev Board (C) | SoM (D) |
|-----------|--------|------------------|-------------|---------------|---------|
| **Cost at Target Volume** | 30% | 9 (lowest at volume) | 6 (moderate) | 5 (high at volume) | 8 (good) |
| **Time to Market** | 25% | 4 (slow) | 6 (moderate) | 10 (fastest) | 7 (good) |
| **Professional Quality** | 20% | 9 (excellent) | 7 (good) | 4 (dev board look) | 9 (excellent) |
| **Design Risk** | 15% | 5 (complex) | 7 (moderate) | 9 (low risk) | 6 (moderate) |
| **Manufacturing Scalability** | 10% | 10 (excellent) | 7 (good) | 4 (limited) | 9 (excellent) |

### Weighted Scores

| Option | Calculation | **Total Score** |
|--------|-------------|-----------------|
| **Option A** | (9×0.30)+(4×0.25)+(9×0.20)+(5×0.15)+(10×0.10) | **7.25** |
| **Option B** | (6×0.30)+(6×0.25)+(7×0.20)+(7×0.15)+(7×0.10) | **6.55** |
| **Option C** | (5×0.30)+(10×0.25)+(4×0.20)+(9×0.15)+(4×0.10) | **6.75** |
| **Option D** | (8×0.30)+(7×0.25)+(9×0.20)+(6×0.15)+(9×0.10) | **7.75** |

---

## Final Decision

### Selected Option: Option D - System-on-Module (SoM) Approach ✅ **RECOMMENDED**

**Decision Score:** 7.75/10 (highest rated option balancing all criteria)

**Final Configuration:**
- **ESP32 SoM:** ESP32-S3-WROOM-1 or equivalent system-on-module
- **Carrier Board:** 4-layer custom PCB optimized for pH balancer application
- **Size:** 85×65mm carrier board fitting within enclosure constraints
- **Assembly:** Professional SMT assembly with automated production capability
- **Total Cost:** $33-80 (low volume), $18-45 (production volume)

**Key Decision Factors:**
1. **Professional Quality:** Custom carrier board enables professional appearance
2. **Regulatory Benefits:** SoM handles ESP32 RF certification requirements
3. **Good Time-to-Market:** Faster than single board, professional quality
4. **Scalability:** Excellent manufacturing scalability for production
5. **Cost Balance:** Good cost at production volumes with reasonable NRE

---

## Recommendation

### Primary Recommendation: Option D - SoM + Carrier Board ✅ **SELECTED**

**Implementation Strategy:**

**Phase 1: Carrier Board Design (4 weeks)**
- Design 4-layer carrier PCB with proper power and signal routing
- Include all necessary interfaces: power, pumps, sensors, connectors
- Optimize layout for EMC compliance and thermal management
- Create comprehensive test point coverage for debugging

**Phase 2: Prototype and Testing (3 weeks)**
- Fabricate prototype carrier boards and assemble with SoM
- Comprehensive testing of all interfaces and functionality
- EMC pre-compliance testing and optimization
- Thermal testing under maximum load conditions

**Phase 3: Production Preparation (2 weeks)**
- Finalize design for manufacturing (DFM) optimization
- Create assembly documentation and test procedures
- Qualify multiple SoM suppliers for supply chain security
- Prepare production test fixtures and procedures

**Carrier Board Design Specifications:**

**PCB Requirements:**
```
Carrier Board Specifications:
- Size: 85×65mm (fits within enclosure)
- Layers: 4-layer stackup for signal integrity
- Material: FR4 with controlled impedance
- Thickness: 1.6mm standard
- Surface Finish: HASL or ENIG for reliability
- Soldermask: Green with white silkscreen
- Via: 0.2mm minimum via size
- Trace: 0.1mm minimum trace width
```

**Component Layout Zones:**
```
Carrier Board Layout:
┌─────────────────────────────────┐
│ Power Input    │ ESP32-S3-SoM   │
│ & Regulation   │ Module Area     │
├─────────────────────────────────┤
│ Pump Relay     │ Sensor         │
│ Drivers        │ Interfaces     │
├─────────────────────────────────┤
│ Status LEDs    │ I/O Connectors │
└─────────────────────────────────┘
```

**SoM Selection Criteria:**
- ESP32-S3 for enhanced performance and security
- Integrated antenna or external antenna option
- Temperature range: -40°C to +85°C
- Multiple supplier options (Espressif, AI-Thinker, others)
- FCC/CE certification included
- Standard footprint for supply chain flexibility

**Manufacturing Considerations:**
- Design for SMT assembly automation
- Include fiducial markers for pick-and-place alignment
- Proper thermal relief for power components
- Test points accessible for in-circuit testing
- Panelization support for efficient production

### Alternative Recommendation for Rapid Prototyping

**Option C for Initial Development:**
For fastest initial development and market testing:
- Start with dev board + shield approach for immediate prototyping
- Develop and test all functionality quickly
- Transition to SoM + carrier board for production
- Use shield design as reference for carrier board layout

This SoM approach provides the optimal balance of professional quality, development speed, and production scalability while maintaining cost-effectiveness for the target market.