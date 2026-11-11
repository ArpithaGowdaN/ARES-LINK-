# ARES-Link
### Autonomous Resilient Edge-Store-and-Forward Interplanetary Link

**Store Intelligently. Prioritise Critically. Transmit Adaptively. Resume Reliably.**

---

## 1. Project Overview

ARES-Link is a proposed adaptive, priority-aware communication framework designed to improve data delivery between planetary surface assets, such as landers and rovers, and orbiting relay spacecraft.

In planetary exploration missions, continuous communication between a lander or rover and Earth may not always be possible because of large distances, limited power, antenna constraints and planetary geometry. Consequently, orbiters are often used as communication relays.

However, communication between a surface asset and an orbiter is available only during specific contact windows. During communication blackouts, generated science data, telemetry and images must be stored onboard until another communication opportunity becomes available.

ARES-Link addresses this challenge through intelligent data prioritisation, link-aware adaptive transmission, selective recovery of interrupted data and progressive image delivery.

---

## 2. Problem Statement

Planetary surface missions generate large volumes of science data, telemetry and images. However, communication opportunities with orbiting relay spacecraft are intermittent and communication conditions may change during a contact window.

Conventional data-transfer approaches alone may not efficiently address all the challenges of:
- Short-duration communication windows.
- Communication blackouts between orbital passes.
- Changing communication link quality.
- Critical alerts waiting behind large routine data files.
- Interrupted transfers requiring recovery of missing data.

The challenge is not simply transmitting data, but ensuring that the most important information is delivered efficiently and reliably whenever communication becomes available.

---

## 3. Existing Solutions and Research Context

Existing space communication systems already provide several mechanisms for handling intermittent connectivity.

### Direct-to-Earth Communication
Spacecraft communicate directly with ground stations when distance, antenna pointing and link-budget conditions permit.

### Orbiter Relay Networks
Planetary rovers and landers transmit data to orbiting spacecraft during scheduled communication opportunities. The orbiter stores and subsequently forwards the data to Earth.

### Delay-Tolerant Networking (DTN)
DTN provides a store-and-forward networking architecture designed for communication environments with long delays and intermittent connectivity.

### Bundle Protocol (BPv7)
The Bundle Protocol supports the exchange of data bundles across disrupted networks.

### CCSDS Proximity-1
CCSDS Proximity-1 provides standardised communication protocols for proximity links between spacecraft and planetary surface assets.

### ARES-Link's Proposed Contribution

ARES-Link does not attempt to replace existing space communication standards. Instead, it focuses on prototyping and evaluating an integrated embedded data-handling strategy combining:

- Anomaly-driven emergency data prioritisation.
- Value of Information (VoI) and Age of Information (AoI)-based scheduling.
- Link-quality-aware adaptive data chunking.
- Selective Acknowledgment (SACK)-based checkpointing.
- Progressive image delivery.
- Live communication and mission-status visualisation.

---

## 4. Real Mission Examples and Communication Challenges

### 4.1 Mars Rover Relay Network

NASA's Mars rovers, including Curiosity and Perseverance, use orbiters as communication relays to send science and engineering data back to Earth.

**Relevance to ARES-Link:**
- Demonstrates the importance of orbiter-based communication.
- Highlights the need to manage onboard data around scheduled relay opportunities.
- Shows why efficient use of available communication windows matters.

### 4.2 Cassini–Huygens Mission (2005)

The Huygens probe descended through Titan's atmosphere and transmitted scientific data to the Cassini spacecraft, which relayed the information to Earth.

A communication configuration problem resulted in the loss of data from one of the probe's two communication channels, including images that were not received.

**Relevance to ARES-Link:**
- Demonstrates the importance of communication-system reliability.
- Highlights the consequences of communication configuration failures.
- Reinforces the need for robust communication planning and data recovery strategies.

### 4.3 Chandrayaan-2 and Chandrayaan-3 (2019–2023)

During Chandrayaan-2, communication with the Vikram lander was lost during its final descent towards the lunar surface.

Following the mission, ISRO incorporated several improvements into Chandrayaan-3, including enhancements to the lander's sensors, software, landing systems and system redundancy. Chandrayaan-3 also included a planned contingency communication link through the Chandrayaan-2 orbiter.

**Relevance to ARES-Link:**
- Demonstrates the importance of reliable communication links in lunar missions.
- Highlights the role of communication redundancy and backup relay paths.
- Reinforces the importance of robust onboard decision-making when communication is disrupted.

**Key takeaway:** These missions demonstrate why resilient communication architectures, reliable onboard systems and effective data-handling strategies are essential for planetary exploration.

---

## 5. Proposed Solution

ARES-Link introduces an intelligent edge-based communication workflow that manages generated data before, during and after an orbiter contact window.

The proposed system operates through four major stages:

1. **Data Processing:** The STM32 processes incoming sensor and mission data and identifies potential anomalies.
2. **Intelligent Prioritisation:** Data is classified and scheduled according to its importance, freshness and urgency.
3. **Adaptive Transmission:** Data is transmitted in dynamically selected chunks according to measured link conditions.
4. **Reliable Recovery:** Selective acknowledgments identify missing chunks, allowing interrupted transfers to resume during subsequent contact windows.

---

## 6. System Architecture

### Lander-Side Node
- STM32 microcontroller for data processing and anomaly detection.
- MPU6050 sensor for motion and vibration monitoring.
- Local flash storage for buffering generated data.
- SX1278 LoRa transceiver for communication with the orbiter-side prototype.

### Orbiter-Side Node
- ESP32 development board for communication and relay-side processing.
- SX1278 LoRa transceiver for receiving data.
- Storage for received data chunks.
- ACK/SACK mechanism for acknowledging received data and identifying missing chunks.

### Ground Station
- Python-based backend.
- FastAPI and Uvicorn for dashboard services.
- pySerial for serial communication.
- WebSockets for live updates.
- Three.js and WebGL for orbital visualisation.
- HTML5 Canvas for progressive image reconstruction and packet visualisation.

---

## 7. Key Features and Innovations

### 7.1 Blackout Data Buffer
Stores telemetry, logs and images in non-volatile memory during communication blackouts.

### 7.2 Anomaly Pre-emption
Detects critical events such as tilt, shock and thermal anomalies and promotes associated alerts to the highest transmission priority.

### 7.3 VoI + AoI Intelligent Scheduling
Prioritises data based on:
- **VoI (Value of Information):** Importance and usefulness of the information.
- **AoI (Age of Information):** Freshness or age of the information.

### 7.4 Three-Tier Priority Queue
Organises generated data into three categories:
- Tier 1: Emergency alerts.
- Tier 2: Science and engineering telemetry.
- Tier 3: Bulk images and routine data.

### 7.5 Link-Aware Adaptive Chunking
Adjusts proposed data chunk sizes according to measured link quality and communication feedback.

### 7.6 SACK-Based Checkpointing
Uses Selective Acknowledgment to identify successfully received and missing data chunks.

### 7.7 Interrupted Transfer Resumption
Resumes data transmission from the last confirmed checkpoint during a subsequent communication window.

### 7.8 Progressive Image Streaming
Transmits a coarse image preview first, followed by higher-detail image information.

### 7.9 Mission-Control Digital Twin
Provides a live visualisation of orbital contact windows, telemetry, queue status and communication progress.

---

## 8. Communication Workflow

1. Science and sensor data are generated at the lander.
2. STM32 processes the data and performs anomaly detection.
3. The priority scheduler evaluates data importance and freshness.
4. Data is stored in local memory.
5. The system checks whether an orbiter contact window is available.
6. If no contact is available, the system waits while preserving buffered data.
7. When contact becomes available, the ESP32 and LoRa communication link transmit data chunks.
8. The orbiter-side node receives and stores the chunks.
9. ACK/SACK feedback identifies successfully received and missing chunks.
10. Missing chunks are retransmitted when communication is available.
11. Once all required chunks are received and verified, the data is reassembled.
12. Progressive image reconstruction and dashboard updates display the delivered information.

---

## 9. Technologies and Tools

| Category | Technologies |
|---|---|
| Lander Controller | STM32 |
| Orbiter Controller | ESP32 |
| Wireless Communication | SX1278 LoRa |
| Motion Sensor | MPU6050 |
| Local Storage | Flash Memory |
| Firmware | Embedded C/C++ |
| Communication Interfaces | SPI, UART |
| Data Scheduling | VoI + AoI |
| Anomaly Detection | Threshold-based and EWMA |
| Backend | Python 3, FastAPI, Uvicorn |
| Serial Communication | pySerial |
| Live Updates | WebSockets |
| Visualisation | Three.js, WebGL |
| Image Reconstruction | HTML5 Canvas |

---

## 10. Prototype Demonstration and Testing

The prototype is intended to demonstrate the proposed communication mechanisms using a short-range LoRa link and simulated orbital contact windows.

Planned demonstrations include:

- Data generation and local buffering.
- Emergency alert prioritisation.
- Comparison of FIFO and priority-based scheduling.
- Adaptive data chunk transmission.
- Simulated communication interruption.
- SACK-based missing-chunk identification.
- Transfer resumption during a subsequent contact window.
- Progressive image reconstruction.
- Live dashboard updates.
- Packet-loss simulation and performance evaluation.

### Proposed Performance Metrics

- End-to-end data delivery latency.
- Emergency alert delivery delay.
- Throughput.
- Packet delivery ratio.
- Number of retransmitted chunks.
- Transfer completion time.
- Recovery efficiency.
- Power consumption.

Performance improvements will be established through experimental comparison with appropriate baseline methods.

---

## 11. Expected Outcomes

- Improved handling of intermittent communication opportunities.
- Priority-based delivery of critical mission information.
- More efficient data transmission under changing link conditions.
- Reduced retransmission of already received data.
- Recovery of interrupted transfers across subsequent contact windows.
- Early availability of image previews.
- Improved visibility of communication and mission status through a live dashboard.

---

## 12. Limitations and Scope

ARES-Link is an embedded communication prototype intended to demonstrate selected mechanisms relevant to intermittent space communication.

- LoRa is used as a short-range experimental communication link, not as an actual interplanetary radio link.
- Orbital contact windows and communication disruptions are simulated for prototype evaluation.
- The prototype does not claim full CCSDS or Bundle Protocol compliance.
- Actual deep-space communication requires specialised radios, antennas, link budgets, coding schemes and space-qualified hardware.
- Performance improvements will be validated through controlled experiments rather than assumed in advance.

---

## 13. Future Scope

- Integration with more advanced DTN and Bundle Protocol implementations.
- Evaluation using hardware-in-the-loop communication simulations.
- Advanced link prediction and adaptive scheduling algorithms.
- Integration with more realistic orbital and communication channel models.
- Exploration of low-power, radiation-tolerant embedded hardware.
- Extension to multi-orbiter and multi-surface-node relay networks.

---

## 14. Project Vision

ARES-Link aims to make every available communication opportunity count.

By prioritising critical information, adapting transmission to changing link conditions and recovering interrupted data across subsequent contact windows, the project explores a more resilient and efficient approach to communication for future planetary exploration missions.

**Store intelligently. Prioritise critically. Transmit adaptively. Resume reliably.**

---

## 15. References and Further Reading

- [NASA Mars Relay Network](https://mars.nasa.gov/msl/mission/communications/)
- [NASA Delay/Disruption Tolerant Networking](https://www.nasa.gov/technology/space-comms/)
- [CCSDS – Consultative Committee for Space Data Systems](https://ccsds.org/)
- [ESA – Cassini-Huygens Mission](https://www.esa.int/Science_Exploration/Space_Science/Cassini-Huygens)
- [ISRO – Chandrayaan-2](https://www.isro.gov.in/Chandrayaan2.html)
- [ISRO – Chandrayaan-3](https://www.isro.gov.in/Chandrayaan3.html)

---

**Project:** ARES-Link  
**Full Name:** Autonomous Resilient Edge-Store-and-Forward Interplanetary Link  
**Domain:** Space Communication | Embedded Systems | Delay-Tolerant Networking | Aerospace Technology
