# System Architecture

## Mechanical Architecture

**Motion system:** CoreXY

**Configuration:**

- Linear rail on X-axis
- Linear rods on Y-axis
- Linear rods on Z-axis

**Extruder:** Direct drive

**Build plate:** Magnetic powder-coated PEI

**Build volume:** 170 × 170 × 150 mm

**Enclosure:** Fully enclosed design

**Current enclosure implementation:**

- 3D printed structural components
- Acrylic doors and panels

**Future manufacturing considerations (under evaluation):**

- Injection molded outer shell
- Cast aluminum internal structure
- Thermoset plastic structure
- Combination approaches

No final manufacturing decisions have been made. The current enclosure is representative of the intended form factor and user experience, but it is not production-ready.

---

## Mechanical Design Decisions

**CoreXY selection rationale:**

CoreXY was selected based on product requirements rather than performance optimization alone.

Key reasoning:

- **Compact packaging:** The CoreXY configuration allows the motors to remain stationary, reducing the moving mass and enabling a smaller machine footprint relative to the build volume.
- **Efficient use of internal volume:** The motion system fits within the enclosure without excessive dead space.
- **Potential for higher-speed operation:** Lower moving mass provides a theoretical path to higher print speeds, aligning with the 200–300 mm/s target.
- **Alignment with product goals:** The compact, self-contained nature of CoreXY supports the approachable, consumer-oriented product identity.

This was a requirements-driven decision. CoreXY was not selected because it is the optimal motion system for all applications, but because it best satisfied the combination of footprint, weight, and speed targets given the product constraints.

---

## Electronics Architecture

**Motion controller:** BTT SKR Mini E3

**Firmware:** Marlin

**User interface controller:** ESP32-S3

**Power architecture:**

- External 24V power supply
- Future intent is laptop-style power brick architecture
- No mains voltage routed inside the machine
- Local voltage regulation performed on control boards

This power architecture was selected to improve safety and simplify regulatory compliance. By keeping mains voltage external, the internal electronics operate at low voltage only, reducing shock hazard and simplifying service access.

---

## Software Architecture

**Marlin (motion controller) responsibilities:**

- Motion control
- G-code execution
- Printer control (temperature, fans, endstops)

**ESP32-S3 (user interface controller) responsibilities:**

- User interface rendering
- Guided workflows
- Connectivity management
- Job selection and presentation
- Status monitoring
- Safety supervision

**Communication between controllers:**

- Transfer print jobs
- Start print
- Pause print
- Cancel print
- Monitor status

The architecture intentionally separates printer control from user experience functionality. Marlin handles real-time motion and thermal control, while the ESP32-S3 handles interaction, connectivity, and higher-level workflow logic. This separation provides several benefits:

- The UI can be updated or replaced without modifying motion firmware
- Safety-critical motion control remains isolated from complex UI code
- Future connectivity features can be added to the ESP32 without impacting real-time printer behavior

---

## User Experience Philosophy

The touchscreen is not merely a printer display. It represents a tablet-inspired interaction model designed to replace traditional 3D printer workflows with guided, user-friendly experiences.

**Goals:**

- Reduce user confusion
- Support children and first-time users
- Provide contextual assistance
- Simplify operation

**Included concepts:**

- **Help functionality:** Contextual guidance available during each workflow step
- **Guided workflows:** Step-by-step operation replacing manual configuration
- **Future mobile integration:** Planned connection to mobile application for remote job management
- **Future cloud integration:** Planned access to cloud-hosted model catalog and content library
<!--
&gt; Placeholder – UI screenshots (home screen, workflow steps, help overlay)  
&gt; Placeholder – UX flow diagrams
-->
---

## Connectivity Roadmap

| Phase | Status | Features |
|-------|--------|----------|
| Current MVP | Implemented | Preloaded models stored locally on the device |
| In development | Active | USB model loading from external storage |
| Future | Planned | Wi-Fi connectivity |
| Future | Planned | Cloud model catalog |
| Future | Planned | Mobile application integration |
| Future | Planned | Remote monitoring |

Features are clearly distinguished by implementation status. No cloud or mobile functionality is currently operational.

---

## Safety Architecture

Safety was treated as a primary system requirement, not an afterthought.

**Hazards considered:**

| Hazard | Source |
|--------|--------|
| Hot nozzle | Extruder operating temperature up to ~250°C |
| Heated build plate | Bed operating temperature up to ~70°C |
| Moving axes | CoreXY motion system during operation |
| Pinch points | Belt paths, door hinges, enclosure gaps |
| Airborne particles | Filament heating and extrusion emissions |
| Unauthorized access | Child opening enclosure during operation |

**Mitigations:**

- **Fully enclosed design:** Contains hot surfaces and moving components
- **Door interlock system:** Prevents operation when enclosure is open
- **Automatic door locking:** Secures door during print jobs
- **Safety validation before operation:** Software checks confirm safe state before motion or heating begins
- **HEPA filtration:** Reduces airborne particle emissions (filter performance not formally validated)
- **Software safety monitoring:** ESP32-S3 supervises printer state and can interrupt operation
- **Controller-level supervision:** Motion controller enforces thermal limits and motion bounds
<!--
&gt; Placeholder – Safety architecture diagram  
&gt; Placeholder – Interlock mechanism photos  
&gt; Placeholder – Filtration subsystem photo
-->
---

## Material Strategy

The system uses standard 3D printing filament. The platform follows an **open-material philosophy** rather than relying on proprietary consumables.

**Recommended material focus:**

- PLA
- PLA+

**Reasoning:**

- Ease of use and reliability for first-time users
- Lower operating temperatures
- Better suitability for educational environments
- Reduced emissions relative to many engineering materials

**Approximate thermal limits:**

- Nozzle: up to approximately 250°C
- Bed: up to approximately 70°C

These are not certified limits. They represent the current hardware configuration and recommended operating envelope. Use of higher-temperature materials would require hardware and safety revalidation.
<!--
&gt; Placeholder – Material compatibility chart
-->
