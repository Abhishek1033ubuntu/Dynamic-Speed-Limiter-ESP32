# Dynamic Speed Limiter for Automobiles (ESP32 + LoRa)

An infrastructure-to-vehicle (I2V) wireless speed-control system that dynamically restricts maximum vehicle speed in designated safety zones (school zones, municipal limits, construction areas) using Sub-GHz RF beacons and Drive-by-Wire throttle interception.

---
This is an 'adaptation' of the system architecture our group (my colleagues and I) submitted to the College as a Final Year Project manadatory for completion of the Bachelor of Engineering Degree from Rajiv Gandhi Proudyogiki Vishwavidyala (RGPV) Bhopal, in 2005 via Maharana Pratap college of Technology Electronics and Communications Branch.
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

⚡ Key Hardware ComponentsMicrocontroller: Robozar ESP32 Development Board (Dual-Core 240MHz)Wireless Transceiver: xcluma SX1278 / SX1276 LoRa Module (433MHz)Digital-to-Analog Converter: Dual MCP4725 12-Bit DACs (I2C Addresses 0x60, 0x61)Signal Buffer: LM358 Automotive Op-Amp (Unity-Gain Configuration)Galvanic Isolation: PC817 Optocoupler (VSS Speed Signal Protection)Power Step-Down: MP1584EN DC-DC Buck Regulator (12V/14.4V to 5V Output)
---
🔒 Circuit Safety & Signal IsolationGalvanic VSS Isolation: The $12\text{V}$ Vehicle Speed Sensor line is decoupled via a PC817 optocoupler, isolating high-voltage transients from the $3.3\text{V}$ ESP32 GPIOs.Thermal Efficiency: A switching buck converter operates at $>90\%$ efficiency, eliminating heat dissipation issues associated with linear regulators.Impedance Matching: The dual MCP4725 DAC outputs pass through an LM358 op-amp buffer to prevent ECU input loading.
---
