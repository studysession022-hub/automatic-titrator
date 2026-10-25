# automatic-titrator
# OpenTitrator
### An Open-Source Automated Titration System

**Low-cost automation · Embedded systems · Computer vision · Analytical chemistry**

OpenTitrator is a student-led engineering project focused on developing a low-cost, automated titration system that combines mechanical fluid dispensing, embedded control, and optical endpoint detection.

The objective is to build a modular system capable of handling sample and titrant delivery, controlling the titration process, detecting colour changes near the endpoint, and presenting the resulting measurements to the user.

Rather than treating titration as a purely manual laboratory procedure, this project explores how accessible electronics, mechanical components, and computer vision can be integrated into a reproducible analytical instrument.

> **Project status:** Prototype development and subsystem testing.

---

## Table of Contents

- [Motivation](#motivation)
- [Project Objectives](#project-objectives)
- [System Overview](#system-overview)
- [Key Subsystems](#key-subsystems)
- [Working Principle](#working-principle)
- [Technology Stack](#technology-stack)
- [Calibration and Data Analysis](#calibration-and-data-analysis)
- [Development Roadmap](#development-roadmap)
- [Current Limitations](#current-limitations)
- [Repository Structure](#repository-structure)
- [Contributing](#contributing)
- [Safety](#safety)
- [License](#license)

---

## Motivation

Conventional titration is a widely used analytical technique, but manual execution requires careful handling, consistent dropwise addition, continuous observation, and operator judgement when identifying the endpoint.

Commercial automated titration systems can reduce manual intervention, but their cost and complexity may limit accessibility for educational laboratories, independent researchers, and resource-constrained institutions.

OpenTitrator investigates an alternative approach: a modular instrument built using accessible components and documented engineering principles.

The long-term goal is not simply to automate liquid dispensing. It is to investigate the accuracy, repeatability, reliability, and practical limitations of an affordable automated analytical system.

## Project Objectives

- **Automated fluid delivery:** Develop motor-driven syringe assemblies for controlled sample and titrant dispensing.
- **Optical endpoint detection:** Investigate camera-based colour analysis to identify endpoint-related changes without relying exclusively on human observation.
- **Embedded control:** Coordinate motors, valves, sensors, and other peripherals through a microcontroller.
- **Repeatable measurements:** Characterise dispensing accuracy, backlash, dead volume, and other sources of measurement error.
- **Data-driven calibration:** Develop and evaluate mathematical models that relate commanded actuator motion to delivered liquid volume.
- **Modular architecture:** Design the system so that individual subsystems can be tested, calibrated, and improved independently.
- **Accessible documentation:** Publish design decisions, test procedures, code, calibration data, and lessons learned wherever possible.

## System Overview

The proposed instrument consists of several interconnected subsystems.

| Subsystem | Function |
|---|---|
| Syringe dispensing mechanism | Converts stepper-motor motion into controlled plunger displacement |
| Motor driver | Supplies the current required to operate the stepper motor |
| Fluid-control valves | Direct liquid flow during filling and dispensing |
| Microcontroller | Coordinates actuator commands and system states |
| Optical chamber | Provides a controlled environment for observing the reaction |
| Camera and image processing | Analyses colour changes during titration |
| Mixing assembly | Maintains mixing during liquid addition |
| User interface | Allows parameter selection and displays system status and results |
| Power system | Provides suitable supply voltages for motors, valves, and control electronics |

The architecture is intended to separate the mechanical, electrical, firmware, and optical components so that each can be developed and evaluated independently.

## Key Subsystems

### 1. Precision Syringe Actuation

A lead-screw mechanism drives a syringe plunger using a stepper motor. The system is designed to translate motor movement into predictable linear displacement.

For an ideal syringe with a uniform internal bore:

\[
V = \pi r^2 x
\]

where:

- \(V\) is the displaced volume,
- \(r\) is the internal radius of the syringe,
- \(x\) is the plunger displacement.

For a lead screw with a lead of \(L\) millimetres per revolution and a motor configured for \(S\) effective steps per revolution:

\[
\Delta V_{\text{step}}
=
\frac{\pi r^2 L}{S}
\]

This provides a theoretical estimate of volume per commanded step. Actual delivery must be measured because mechanical compliance, backlash, fluid properties, leakage, and valve behaviour can introduce deviations.

### 2. Fluid Handling

Solenoid valves are being incorporated to control the direction and timing of liquid flow.

The fluid-handling subsystem must account for factors including:

- Residual liquid within tubing and valves.
- Dead volume and trapped air.
- Leakage and incomplete valve closure.
- Pressure variation during dispensing.
- Plunger friction and mechanical backlash.
- Repeatability across repeated filling and dispensing cycles.

These effects will be evaluated experimentally rather than assumed to be negligible.

### 3. Optical Endpoint Detection

A controlled optical chamber is intended to provide consistent illumination and a stable background for observing the reaction mixture.

The proposed computer-vision approach will investigate colour features extracted from camera images, potentially including RGB values, HSV colour-space measurements, and changes relative to a baseline.

The endpoint-detection algorithm will need to distinguish meaningful reaction-related colour changes from variations caused by lighting, reflections, mixing, camera exposure, and the physical appearance of the solution.

**Important distinction:** A detected colour transition is not automatically proof of the chemically correct endpoint. The method must be validated against appropriate reference measurements and known reaction conditions.

### 4. Embedded Control

The microcontroller will coordinate the dispensing mechanism, valves, mixing, sensor acquisition, and user interface.

The control software is intended to support a structured sequence of operations, including initialisation, preparation, dispensing, observation, endpoint evaluation, and termination.

Fault handling and safe stopping behaviour are important parts of the design, particularly where motor motion and electrically actuated valves interact.

---

## Working Principle

The intended operating sequence is:

1. **Configuration:** The user selects the relevant operating parameters, including the sample volume and titration settings.
2. **Preparation:** The system initialises its actuators and sensors and prepares the fluid-handling mechanism.
3. **Sample delivery:** The configured sample volume is dispensed into the reaction vessel.
4. **Mixing:** The mixing mechanism operates to maintain a sufficiently uniform reaction mixture.
5. **Titrant addition:** The titrant is delivered in controlled increments.
6. **Optical observation:** Images of the reaction mixture are acquired under controlled lighting.
7. **Endpoint evaluation:** The image-processing algorithm evaluates whether the configured endpoint criterion has been met.
8. **Termination:** Titrant delivery is stopped when the endpoint condition is satisfied or a fault condition occurs.
9. **Result presentation:** The system reports the recorded measurements and calculated result, provided the relevant analytical method has been validated.

The exact sequence and control logic may evolve as the prototype is tested.

## Technology Stack

The following technologies represent the planned or currently considered implementation. The final stack will be updated as individual subsystems are verified.

| Area | Technology |
|---|---|
| Embedded control | Arduino-compatible microcontroller / ESP32 |
| Firmware | C/C++ |
| Actuation | Stepper motors and motor drivers |
| Linear motion | Lead screw, anti-backlash nut, linear guidance |
| Fluid control | Syringes, tubing, solenoid valves |
| Optical sensing | Webcam or compatible camera |
| Computer vision | Python and OpenCV |
| Calibration | Experimental measurements, regression, statistical analysis |
| Mechanical prototyping | 3D-printed components and fabricated supports |
| Documentation | Git and GitHub |

## Calibration and Data Analysis

Calibration is a central part of the project because commanded motor motion does not guarantee an identical delivered liquid volume under every operating condition.

The calibration process will investigate the relationship between actuator commands and experimentally measured volume.

Potential methods include:

- **Gravimetric calibration:** Estimate delivered volume from measured liquid mass and an appropriate density value.
- **Linear regression:** Establish a baseline relationship between commanded motion and delivered volume.
- **Residual analysis:** Identify systematic errors and regions where a model performs poorly.
- **Repeatability analysis:** Evaluate variation across repeated dispensing trials.
- **Error analysis:** Quantify absolute error, relative error, bias, and measurement uncertainty.
- **Model validation:** Compare predictions against independent measurements not used to fit the model.
- **Edge-case testing:** Investigate backlash, missed steps, plunger limits, valve dead volume, and other abnormal operating conditions.

More complex models will only be considered where the experimental data justifies them. Calibration performance will be assessed using independent test measurements rather than training fit alone.

The resulting calibration procedure, datasets, model assumptions, and performance metrics will be documented as development progresses.

## Development Roadmap

- [x] Define the initial system concept and major subsystems.
- [x] Establish a preliminary mechanical and electrical architecture.
- [ ] Complete the first syringe-actuation assembly.
- [ ] Verify stepper-motor motion and driver behaviour.
- [ ] Test syringe filling and dispensing independently.
- [ ] Characterise fluid-handling losses and repeatability.
- [ ] Implement basic microcontroller coordination.
- [ ] Build and test the optical chamber.
- [ ] Develop an initial colour-analysis pipeline.
- [ ] Integrate optical observations with titrant control.
- [ ] Collect calibration and validation datasets.
- [ ] Evaluate endpoint detection against a suitable reference method.
- [ ] Quantify measurement accuracy and repeatability.
- [ ] Document an integrated prototype and its limitations.

*Checklist items reflect development intent and should be updated as each milestone is demonstrated experimentally.*

## Current Limitations

OpenTitrator is under active development. Its intended capabilities should not be confused with experimentally demonstrated performance.

Current areas requiring investigation include:

- Mechanical dispensing accuracy and repeatability.
- Valve timing, leakage, and residual liquid volume.
- Optical sensitivity to lighting and solution appearance.
- Endpoint detection reliability across different reactions.
- Synchronisation between dispensing, mixing, and image acquisition.
- Calibration stability over repeated operating cycles.
- Analytical accuracy relative to a validated reference method.

Initial testing will focus on establishing a reliable dispensing subsystem before attempting to demonstrate a fully integrated automated titration process.

## Repository Structure

The repository will gradually separate the project's hardware, firmware, software, experimental data, and documentation.

```text
OpenTitrator/
├── README.md
├── docs/
│   ├── system-architecture.md
│   ├── mechanical-design.md
│   └── calibration-methodology.md
├── hardware/
│   ├── schematics/
│   ├── mechanical-design/
│   └── bill-of-materials/
├── firmware/
│   ├── motor-control/
│   └── system-control/
├── software/
│   └── vision/
├── data/
│   ├── raw/
│   └── processed/
├── experiments/
│   └── protocols/
└── LICENSE
```

This is a proposed structure; directories should be added as their corresponding files become available.

## Contributing

Contributions, constructive criticism, experimental feedback, and reproducible testing are welcome.

Areas where contributions may be useful include:

- Stepper-motor control and motion planning.
- Low-cost fluid-delivery mechanisms.
- Computer vision and optical endpoint detection.
- Experimental design and statistical calibration.
- Electronics, circuit protection, and fault handling.
- Mechanical design and fabrication.
- Analytical chemistry and validation methodology.

When submitting a contribution, please include enough information to reproduce the work, along with relevant measurements, assumptions, and known limitations.

## Safety

This project is an experimental instrument and is not a certified analytical device.

- Use appropriate electrical protection for motors, solenoid valves, and power supplies.
- Verify driver current limits and supply ratings before operating the hardware.
- Keep liquids away from exposed electronics.
- Use suitable personal protective equipment and established laboratory procedures for the chemicals involved.
- Test fluid delivery with an appropriate safe liquid before introducing reactive or hazardous chemicals.
- Never rely solely on an unvalidated optical algorithm for safety-critical or regulated measurements.

## License

The project license has not yet been selected. A license should be added before the repository is presented as an open-source hardware and software project. Hardware designs, firmware, software, and documentation may require different licensing choices.

---

## Project Philosophy

OpenTitrator is an exploration of how mechanical engineering, embedded systems, computer vision, and experimental science can come together to create accessible laboratory instrumentation.

The emphasis is on **measurable performance, reproducible experiments, transparent limitations, and iterative engineering**—not simply making a prototype appear to work.

The long-term direction is to build, test, document, and improve an automated titration platform whose design and experimental findings can be examined and extended by others.


