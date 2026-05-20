# **📚 Thrust Rocket Guide**

### **Brief Overview**

This note covers **Thrust Vectored Rocketry** and was created from the [How To Build a Thrust Vectored Model Rocket - National Rocketry Conference 2020](https://www.youtube.com/watch?v=4cw9K9yuIyU) YouTube video. It covers thrust vector control fundamentals, model rocket design, flight computer architecture, PID control, and state machine logic.

### **Key Points**

- Understand the basic principles of thrust vectoring and why it’s critical for model rockets.
    
- Learn how to design a simple gimbal mount and choose appropriate motors and materials.
    
- Get a clear grasp of the flight computer’s hardware components and software architecture.
    
- Apply PID control theory and state machines to maintain stability and safely deploy parachutes.
    

---

## **🚀 Thrust Vector Control (TVC) Fundamentals**

> **Thrust Vector Control** is a **technique** used in the aerospace industry to stabilize craft—not a physical object. It is employed for airplanes, fighter jets, and rockets.

### **How TVC Works**

The core principle involves manipulating the **thrust vector** (the direction of engine exhaust) to generate control forces:

|   |   |   |
|---|---|---|
|Condition|Thrust Vector|Result|
|Normal flight|Straight down|Reaction force straight up|
|Tilted/perturbed flight|Angled to the side|Resolved into vertical + small lateral force|

That **small lateral force vector** is what enables active stabilization.

---

## **🔧 Methods of Achieving TVC**

|   |   |   |
|---|---|---|
|Method|Description|Example|
|**Gimbaling the motor**|Dual-axis hinge allows motor to pivot|Joe Barnard's BPS approach|
|**Gimbaling the nozzle**|Nozzle itself pivots|Space Shuttle SRB ground tests|
|**Jet vanes**|Small fins inserted into exhaust stream|V-2 rocket, Copenhagen Suborbitals|
|**Liquid Injection TVC (LiTVC)**|Inject reactive/inert gas into exhaust|Boston University Rocket Propulsion Group|
|**Thrust paddles**|Small paddles deflect exhaust|Florida Tech Vector Bravo system|

---

## **🛠️ Getting Started with Model Rocket TVC**

### **Start Small**

> **Critical advice:** Begin with **small motors**, not large ones. The first attempt rarely goes perfectly—failure with an F motor is far more forgiving than with an M motor.

**Recommended specifications for beginners:**

|   |   |
|---|---|
|Parameter|Recommendation|
|**Vehicle mass**|500–1,000 grams|
|**Motor class**|Low-power (F, G range)|
|**Airframe diameter**|66mm (BT-80), 74mm (3"), or 98mm (4")|
|**Material**|Cardboard (forgives modifications)|
|**TVC method**|Gimbal mount (not LiTVC at this scale)|

---

## **⚙️ Gimbal Mount Designs**

### **Layered Dual-Servo Approach (BPS Design)**

The motor mount connects to a **bottom servo** (one axis), while a **top servo** serves as the "outer layer" that gimbals the entire bottom assembly—**decoupling the axes**.

### **Alternative Approaches**

|   |   |   |
|---|---|---|
|Design|Description|Advantage|
|**Orthogonal rod system**|Two 90° rods push/pull on motor|Mimics liquid engine gimbaling|
|**Compliant gimbal**|Rubber/plastic flexure, no metal hinges|No exhaust degradation of metal components|

---

## **🔄 The Importance of Iteration**

> **Expect failure.** Multiple attempts are normal and necessary. Joe Barnard's first 1.5–2 years included many unsuccessful flights alongside successful ones.

Design files are available for download on the **BPS website**.

---

## **🧠 Flight Computer Architecture**

### **Why Avoid Pre-built Systems?**

Pixhawk, ArduPilot, APM, and similar **all-in-one flight control systems** work well for model airplanes but are **not recommended for TVC rockets**.

> **Better approach:** Full control over **software** and **hardware** for maximum efficiency.

---

## **📋 Core Flight Computer Components**

|   |   |   |
|---|---|---|
|Component|Function|Connection|
|**Microcontroller (μC)**|Runs all code|Central hub|
|**IMU** (Inertial Measurement Unit)|Senses orientation and motion via accelerometers + gyroscopes|→ Microcontroller|
|**Barometer**|Senses altitude above ground|→ Microcontroller|
|**TVC Servos**|Execute thrust vectoring commands|→ Microcontroller|
|**Parachute deployment**|Safe recovery system|→ Microcontroller|
|**Battery**|Powers all components|Power bus|
|**State indicators** (buzzers, LEDs)|Quick status check at launch pad|→ Microcontroller|
|**Data recorder** (flash chip/SD)|Post-flight analysis|→ Microcontroller|

> **Note:** Lidar and radar can measure altitude more accurately but are difficult to keep pointed at the ground during flight—especially if the rocket becomes unstable.

---

## **🔌 Microcontroller Options**

|   |   |   |
|---|---|---|
|Board|Characteristics|Recommendation|
|**Raspberry Pi**|Very powerful, full computer|❌ Overpowered, not recommended|
|**BeagleBone**|Powerful, real-time OS|❌ Overpowered, not recommended|
|**Arduino Uno**|Common, familiar|⚠️ Basic option|
|**Teensy 3.2**|Excellent power/price/footprint ratio|✅ **Best option**|
|**ESP8266**|Wi-Fi enabled|Viable alternative|
|**Nucleo (STM32)**|Professional-grade|Viable alternative|

### **Recommended Specific Boards**

|   |   |
|---|---|
|Board|Notes|
|**Arduino Micro / Nano**|Functionally similar; slightly low processing power but workable|
|**Arduino Zero**|Excellent; basis for many custom designs|
|**Teensy 3.2**|Unbeatable processing power for price and footprint|

---

## **📡 Sensor Recommendations**

### **Inertial Measurement Units (IMUs)**

|   |   |   |
|---|---|---|
|Sensor|Features|Notes|
|**Bosch BNO055**|Onboard fusion software, higher quality gyros/accelerometers|More expensive, easier orientation output|
|**MPU-6050**|Standard 6-axis|Reliable, widely used|
|**LSM6DS3**|6-axis|Good alternative|

> **All are MEMS (Micro-Electro-Mechanical Systems)** — cellphone-grade sensors. **Do not use on M motors or larger.**

### **Barometric Pressure Sensors**

|   |   |
|---|---|
|Sensor|Notes|
|**BMP280**|Excellent, modern|
|**MPL3115A2**|Good alternative|
|**BMP180**|Classic, well-known|

> All are MEMS and work fine for model rocket altimetry.

---

## **⚡ Parachute Deployment: MOSFETs vs. Relays**

> **NEVER use relays for parachute deployment on rockets.**

|   |   |   |
|---|---|---|
|Aspect|Relay|MOSFET|
|**Mechanism**|Electromechanical switch (physical contacts, solenoid/magnetic core)|Solid-state, no moving parts|
|**Vibration resistance**|❌ **Risk of accidental triggering under high vibration/shock**|✅ Immune to vibration issues|
|**Suitability**|Ground-side only (launch pads)|✅ **Recommended for flight**|

> **MOSFETs** function as "digital relays" — capable of switching high currents/loads without physical moving parts.

---

## **🔋 Power Systems**

|   |   |   |
|---|---|---|
|Battery Type|Characteristics|Recommendation|
|**9V battery**|Low current capacity|⚠️ Limited for high-demand systems|
|**Lithium Polymer (LiPo)**|High current/voltage output, compact|✅ **Recommended** — "bad boy of batteries"|

> **LiPo warning:** High performance comes with fire risk — handle with proper safety precautions.

---

## **💾 Data Recording Strategy**

|   |   |   |
|---|---|---|
|Approach|Risk|Recommendation|
|**Direct SD card write**|Electromechanical connection — risk of disconnection or partial corruption under vibration/shock|❌ Not recommended for flight|
|**Flash chip (soldered)**|Solid-state, vibration-immune|✅ **Recommended for in-flight**|
|**EEPROM (onboard processor)**|Limited capacity|Viable for small data sets|
|**Post-flight SD transfer**|Safe ground operation|Write to SD after landing detection|

> **Recommended workflow:** Write to flash chip during flight → detect landing → transfer to SD card for easy data retrieval.

---

## **🧩 Assembly Approaches**

|   |   |   |
|---|---|---|
|Method|Characteristics|Best For|
|**Breadboard / Proto board**|Open concept, forgiving of mistakes, easy to modify|**Beginners, first projects, schematic development**|
|**Printed Circuit Board (PCB)**|Lighter, space-efficient, total schematic control|**Final designs, weight-critical applications**|

> **Joe Barnard's experience:** First 1.5–2 years of successful and unsuccessful flights used breadboard/proto board — it works.

---

## **🧠 Software Architecture: State Machines**

> **State machines** simplify code by chunking it into discrete sections (states), running only relevant code for each flight phase.

### **Flight States**

|   |   |
|---|---|
|State|Description|
|**Ground Idle**|On launch pad, pre-launch|
|**Powered Flight**|Under thrust, TVC active|
|**Unpowered Flight**|Motor burned out, coasting upward|
|**Ballistic Descent**|Past apogee, before parachute deployment|
|**Chute Descent**|Under parachutes|
|**Landing/Safe**|On ground, mission complete|

### **State Transitions**

|   |   |   |
|---|---|---|
|From|To|Trigger Condition|
|Ground Idle → Powered Flight|**Liftoff detection**||
|Powered Flight → Unpowered Flight|**Burnout detection**||
|Unpowered Flight → Ballistic Descent|**Apogee detection**||
|Ballistic Descent → Chute Descent|**Pyro fire (parachute deployment)**||
|Chute Descent → Landing/Safe|**Altitude < 5m for ~10 seconds**||

---

## **🔍 Detection Algorithms**

### **Liftoff Detection**

**Primary sensor:** Z-axis accelerometer (longitudinal axis)

**Logic:**

- If acceleration threshold → potential liftoff
    
- **Critical safeguard:** Require sustained acceleration for **0.1 seconds** to confirm
    
- Prevents false triggering from accidental drops/shocks
    

### **Burnout Detection**

**Logic:** Reverse of liftoff detection

- If acceleration 2 m/s² → potential burnout
    
- Again, require sustained condition (~0.1s) before state transition
    

> **Real-world parallel:** Rocket Lab's Electron uses similar burnout detection ("burnout detect mode") due to electric turbo pumps burning tanks to depletion without precise propellant sensors.

### **Apogee Detection**

**Approach:** Compare altitude readings separated by time interval

**Example:**

- Current altitude: 30 meters
    
- Altitude 1 second ago: 25 meters
    
- Since → still ascending (or at apogee)
    

When current altitude < previous altitude → apogee passed, switch to ballistic descent

## **🎯 Apogee Detection Algorithm**

Building on the altitude comparison method discussed earlier, apogee detection compares current altitude against previous readings:

|   |   |   |
|---|---|---|
|Condition|Interpretation|Action|
|Current altitude > Altitude 1 second ago|Still ascending|Continue unpowered flight state|
|Current altitude < Altitude 1 second ago|Descending|Transition to ballistic descent|

> **Alternative approaches:** Velocity and acceleration can also detect apogee, but MEMS sensor quality makes altitude comparison more **robust** — especially as flight profiles scale larger.

---

## **🪂 Parachute Deployment Detection**

### **Deployment Trigger Logic**

|   |   |   |
|---|---|---|
|Flight Phase|Altitude Condition|Action|
|Ballistic Descent|Altitude 25 meters (configurable)|Fire pyrotechnic charges → Transition to chute descent|

> **Critical state machine advantage:** This code **only runs in ballistic descent state**. Running it during powered flight would deploy chutes on ascent — catastrophic result.

---

## **🛬 Landing Detection**

### **Primary Method: Altitude-Based**

|   |   |   |
|---|---|---|
|Condition|Threshold|Duration|
|Altitude 5 meters|Sustained|$\sim$10 seconds|

### **Alternative Detection Methods**

|   |   |   |
|---|---|---|
|Sensor|Approach|Principle|
|**Gyroscopes**|Zero movement detection|Under parachutes, rocket always rolls/moves; on ground, standard deviation drops to near-zero|
|**Accelerometers**|Gravity vector detection|Combined axes should read when stationary|
|**Barometer**|Constant altitude|No altitude change over time window|

> **Key insight:** Multiple valid approaches exist — choose based on sensor availability and noise characteristics.

---

## **🧭 GNC: Guidance, Navigation, and Control**

> **GNC** encompasses the systems responsible for keeping rockets pointed correctly and on trajectory.

### **The Stability Challenge**

TVC rockets lack **aerodynamic stability** — they are essentially "big long sticks" without fins to provide passive stability. The GNC system must actively maintain attitude.

### **Falcon 9 Landing: G-FOLD Algorithm**

|   |   |
|---|---|
|Aspect|Description|
|**Name**|G-FOLD: **G**uidance for **F**uel **O**ptimal **L**arge **D**iverts|
|**Function**|Solves fuel-optimal and minimum landing error problems|
|**Mathematical approach**|Equivalent convex relaxation of non-convex spacecraft control constraints|
|**Key technique**|Successive convexification|

> **Simplified reality:** The full G-FOLD mathematics are highly complex; conceptual understanding of feedback loops proves more practical for most applications.

---

## **🔁 Control Theory: The PID Controller**

### **System Components Conceptual Model**

|   |   |   |   |
|---|---|---|---|
|Block|Input|Output|Physical Meaning|
|**TVC Mount**|Command angle (degrees)|Torque|Servo dynamics, misalignment, delays|
|**Rocket**|Torque (N·m or lb·ft)|Angle (degrees)|Vehicle rotational dynamics|

### **Feedback Loop Structure**

The angle output feeds back to become the command input, creating a **closed-loop system**:

When the rocket tilts, the TVC mount tilts proportionally, generating corrective torque that returns the vehicle toward zero angle.

---

## **⚙️ Proportional Control (P-Gain)**

### **The P-Box: Proportional Multiplier**

|   |   |   |
|---|---|---|
|Parameter|Value|Effect|
||0.5|Output = input angle|

**Example calculations:**

- Input: → Output: command
    
- Input: → Output: command
    
- Input: → Output: command
    

> **Purpose:** allows tuning correction strength — critical for matching control authority to vehicle mass and thrust.

### **The Overcorrection Problem**

|   |   |
|---|---|
|Scenario|Result|
|Light rocket (500g) + Large motor (M class)|Severe overcorrection, instability|

Proportional-only control cannot "brake" — it only scales response magnitude.

---

## **🚗 Conceptual Analogy: The Parking Lot Problem**

|   |   |   |
|---|---|---|
|Element|Car Scenario|Rocket Equivalent|
|Control variable|Position (distance to parking spot)|Angle (deviation from vertical)|
|Proportional response|Accelerator pedal position|TVC mount command|
|Missing element|**Brake** (velocity control)|**Derivative term** (rate control)|

> **Critical insight:** Braking controls **velocity**, not position directly. The derivative term provides equivalent rate feedback.

---

## **🔄 Derivative Control (D-Gain)**

### **The D-Box: Rate Feedback**

|   |   |   |
|---|---|---|
|Component|Function|Mathematical Representation|
|Derivative block|Computes rate of change||
|Gain multiplier|Scales derivative||

**Calculation chain:**

### **D-Gain in Action**

|   |   |   |   |
|---|---|---|---|
|Condition|Angle|Angular Rate|D-Term Contribution|
|Near zero, moving fast|||extra correction|
|Near zero, stationary|||extra correction|

> **The D-term is the "brake"** — it dampens rapid angular rates regardless of current angle magnitude.

---

## **∫ Integral Control (I-Gain)**

### **The I-Box: Error Accumulation**

|   |   |   |
|---|---|---|
|Parameter|Value|Function|
||0.2|Multiplies accumulated error|

**Accumulation mechanism:**

|   |   |   |
|---|---|---|
|Time Step|Angle|Running Integral|
||||
||||
||||
||||

### **Purpose: Correcting Systematic Errors**

> **Real-world problem:** Perfect mechanical alignment is **impossible**. Mount misalignment of causes steady-state error.

|   |   |
|---|---|
|Without I-Gain|With I-Gain|
|Stabilizes around offset|Accumulates error over time → generates correction command|
|P and D terms cancel, but at wrong angle|Slowly "pushes" system toward true zero|

### **The Danger of I-Gain**

|   |   |
|---|---|
|Scenario|Consequence|
|too high with accumulated error of|Massive correction command → violent oscillation ("fishtailing")|
|Integral windup during saturation|System becomes unstable when returning to normal operation|

> **Critical warning:** The I-gain is the most dangerous term — tune conservatively.

---

## **🎛️ The Complete PID Controller**

### **Architecture Summary**

|   |   |   |   |   |
|---|---|---|---|---|
|Term|Input|Gain|Output Contribution|Role|
|**P** (Proportional)|Current angle|||Immediate response strength|
|**I** (Integral)|Accumulated error|||Eliminate steady-state error|
|**D** (Derivative)|Rate of change|||Dampen oscillations|

### **Final Control Command**

> **Industry relevance:** Multiple launch companies use direct PID controllers for rocket guidance — though many alternative approaches exist.

---

## **🔧 Practical Implementation: Tuning and Simulation**

### **Critical Undiscussed Topics**

|   |   |   |
|---|---|---|
|Topic|Importance|Approach|
|**PID gain tuning**|Essential for stability|Build **offline physics simulation**|
|**Code implementation**|Core functionality|No complete tutorials exist — requires independent development|

### **Simulation Strategy**

|   |   |
|---|---|
|Component|Purpose|
|Physics engine|Model rocket dynamics, thrust, aerodynamics|
|Sensor models|Add realistic noise to simulated IMU/barometer data|
|Control loop|Test PID implementation before flight|
|Monte Carlo runs|Verify robustness across parameter variations|

> **Joe Barnard's experience:** First 1.5–2 years included many unsuccessful flights alongside successful ones — iteration is expected.

---

## **🚀 Advanced Topics and Future Directions**

### **Unpowered Flight Stability**

|   |   |   |
|---|---|---|
|Challenge|Physics|Mitigation|
|Post-burnout instability|No thrust for TVC, aerodynamic forces dominate|Proper stability margin design; consider active fin control|

### **Active Fin Control (Experimental)**

|   |   |
|---|---|
|Aspect|Challenge|
|Aerodynamic characterization|Airfoil behavior varies dramatically with airspeed|
|Control complexity|Commanded torque → actual torque depends on freestream velocity|

### **Current BPS Projects**

|   |   |   |
|---|---|---|
|Project|Description|Goal|
|**Sprint**|Small-fins rocket with CP at CM|Reach 1 km altitude|
|**Sprite**|Hopper vehicle|Precise GNC demonstration|
|**H13 motor**|14–16 second burn, 13 N average thrust|Enable low-and-slow orbital-class simulation|

---

## **📦 BPS Signal Kits**

|   |   |
|---|---|
|Aspect|Details|
|**Availability**|Roughly every 6 months|
|**Constraint**|Single-person operation (living room-based)|
|**Next release**|Expected spring (announced on BPS social media)|

---

## **🛠️ Reaction Control System (RCS) — On Hold**

|   |   |
|---|---|
|Component|Issue|
|**Valves**|Insufficient mass flow rate without expensive large-orifice options|
|**Gas source**|Paintball tanks (air/CO₂); CO₂ preferred for density but thermal concerns|

### **Why Development Paused**

|   |   |
|---|---|
|Factor|Decision|
|Legacy code base|2017-era GNC software needed complete rewrite|
|Strategic priority|Focus on foundational GNC improvements (Sprite vehicle)|
|Future readiness|Better to revisit RCS with modernized control architecture|

---

## **🎓 Key Takeaways**

|   |   |
|---|---|
|Area|Core Principle|
|**TVC implementation**|Start small, iterate, expect failure|
|**State machines**|Chunk code by flight phase for clarity and safety|
|**PID control**|P for response, D for damping, I for error correction — tune carefully|
|**Hardware**|Breadboards work; PCBs for final designs|
|**Software**|Build simulations; no complete tutorials exist|
|**Sensors**|MEMS adequate for small rockets; larger vehicles need professional-grade|