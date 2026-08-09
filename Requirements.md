
⚡ Key Hardware Components
	Microcontroller: Robozar ESP32 Development Board (Dual-Core 240MHz)
	Wireless Transceiver: xcluma SX1278 / SX1276 LoRa Module (433MHz)
	Digital-to-Analog Converter: Dual MCP4725 12-Bit DACs (I2C Addresses 0x60, 0x61)
	Signal Buffer: LM358 Automotive Op-Amp (Unity-Gain Configuration)
	Galvanic Isolation: PC817 Optocoupler (VSS Speed Signal Protection)
	Power Step-Down: MP1584EN DC-DC Buck Regulator (12V/14.4V to 5V Output)
🔒 Circuit Safety & Signal Isolation
	Galvanic VSS Isolation: The 12"V"  Vehicle Speed Sensor line is decoupled via a PC817 optocoupler, isolating high-voltage transients from the 3.3"V"  ESP32 GPIOs.
	Thermal Efficiency: A switching buck converter operates at >90% efficiency, eliminating heat dissipation issues associated with linear regulators.
	Impedance Matching: The dual MCP4725 DAC outputs pass through an LM358 op-amp buffer to prevent ECU input loading.
🚀 Getting Started
	Install Arduino IDE v2.x.
	Add ESP32 board support via Preferences > Additional Boards Manager URLs:
	https://raw.githubusercontent.com/espressif/arduino-esp32/gh-pages/package_esp32_index.json
	Install required libraries via Library Manager:
	Adafruit MCP4725
	LoRa by Sandeep Mistry / RadioLib
	Open src/main.cpp, select your ESP32 board port, and click Upload.
