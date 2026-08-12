# Dynamic Speed Limiter for Automobiles (ESP32 + LoRa)


[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.21863340.svg)](https://doi.org/10.5281/zenodo.21863340)
![Status](https://img.shields.io/badge/Status-Research_POC-orange) ![Type](https://img.shields.io/badge/Type-Simulation_Model-blue)

---

An infrastructure-to-vehicle (I2V) wireless speed-control system that dynamically restricts maximum vehicle speed in designated safety zones (school zones, municipal limits, construction areas) using Sub-GHz RF beacons and Drive-by-Wire throttle interception.

---
This is an 'adaptation' of the system architecture our group (my colleagues and I) submitted to the College as a Final Year Project mandatory for completion of the Bachelor of Engineering Degree from Rajiv Gandhi Proudyogiki Vishwavidyala (RGPV) Bhopal, in 2005 via Maharana Pratap college of Technology Electronics and Communications Branch.

---
**Note on References & IP: Detailed citations and literature references are restricted to protect Intellectual Property. See References.md for details or to request access.

---
## 🛠 System Architecture Diagram

```mermaid
flowchart TD
    subgraph Roadside_Infrastructure ["Roadside Infrastructure"]
        TX[LoRa Beacon Transmitter] -->|Sub-GHz 433MHz RF Signal| RX
    end

    subgraph Vehicle_Interception_System ["Vehicle Interception Unit"]
        RX[xcluma SX1278 LoRa Module] -->|SPI Bus| MCU[ESP32 Microcontroller]
        
        VSS[Vehicle Speed Sensor - 12V] -->|12V Pulse| OPTO[PC817 Optocoupler]
        OPTO -->|3.3V Isolated Pulse| MCU
        
        PWR[14.4V Car Battery] -->|12V-14.4V| BUCK[MP1584EN Buck Converter]
        BUCK -->|Clean 5V Rail| MCU
        BUCK -->|Clean 5V Rail| DAC
        
        PEDAL[Accelerator Pedal Unit] -->|Dual Analog Track A/B| MCU
        
        MCU -->|I2C Bus| DAC[Dual MCP4725 DACs]
        DAC -->|Raw Analog Voltage| OPAMP[LM358 Op-Amp Buffer]
    end

    subgraph Engine_Control ["Vehicle ECU"]
        OPAMP -->|Clamped Analog Voltage| ECU[Engine Control Unit]
    end

    style MCU fill:#2b6cb0,stroke:#333,stroke-width:2px,color:#fff
    style RX fill:#319795,stroke:#333,stroke-width:2px,color:#fff
    style OPTO fill:#d69e2e,stroke:#333,stroke-width:2px,color:#fff
    style BUCK fill:#e53e3e,stroke:#333,stroke-width:2px,color:#fff
