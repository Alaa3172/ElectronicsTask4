# ESP32 with Relay, LDR, and PIR Sensor

This project demonstrates how to connect and control a relay using an ESP32 based on inputs from a Light Dependent Resistor (LDR) and a Passive Infrared (PIR) sensor.

## Table of Contents
- [Introduction](#introduction)
- [Components Required](#components-required)
- [Circuit Diagram](#circuit-diagram)
- [Wiring Instructions](#wiring-instructions)
- [Connection](#connection)
- [Code Example](#code-example)
- [How It Works](#how-it-works)
- [Safety Precautions](#safety-precautions)

## Introduction
This project utilizes an ESP32 microcontroller to read inputs from an LDR (to detect light intensity) and a PIR sensor (to detect motion). Based on these inputs, the ESP32 controls a relay, which can be used to switch on or off an AC or DC load, such as a light or motor.

## Components Required
- ESP32 Development Board
- Relay module (3.3V logic compatible)
- Light Dependent Resistor (LDR)
- Passive Infrared (PIR) Sensor (3.3V compatible)
- 10k ohm resistor (for the LDR)
- Breadboard and jumper wires
- Power supply (usually, the ESP32 is powered via USB)

### ESP32
The ESP32 is a microcontroller developed by Espressif Systems. It is known for having built-in Wi-Fi and Bluetooth, making it ideal for a wide range of Internet of Things (IoT) applications. The ESP32 integrates a dual-core CPU, numerous GPIOs, and various communication interfaces.

__Key Features__
* __Dual-Core Processor__: The ESP32 features a dual-core microprocessor that operates at 240 Mhz.
* __Memory__:
  * _RAM_: 520 KB of internal SRAM.
  * _Flash_: Typically comes with 4 MB of external flash memory.
* __Connectivity__:
  * _Wi-Fi_: IEEE 802.11 b/g/n.
  * _Bluetooth_: Bluetooth Classic (BR/EDR) and Bluetooth Low Energy (BLE).
* __GPIOs__:
  * _Total GPIO Pins_: 34 GPIO pins (PWM, ADC, DAC).
  * _Analog Inputs_: 18 channels with a 12-bit ADC for measuring analog signals.
  * _Digital Outputs_: Capable of generating digital signals.
* __Communication Interfaces__:
  * _I2C_
  * _SPI_
  * _UART_
  * _CAN_
* __PWM__: Pulse Width Modulation outputs for controlling motors, LEDs, etc.
* __Power Supply__: 3.3V operating voltage. Directly powered through the 3.3V pin or regulated using an external 5V supply.
* __Development and Programming__: Can be programmed using the ESP-IDF (Espressif IoT Development Framework), Arduino IDE, or other compatible software.

* __Pinout and GPIO Functions__:

  The ESP32 offers a variety of GPIOs that can be configured for different functions. Key pins include:
  * _GPIO0_: Used for boot mode selection and can also be used as a general-purpose input/output.
  * _GPIO1 (TXD0)_: Transmit pin for UART0.
  * _GPIO3 (RXD0)_: Receive pin for UART0.
  * _GPIO21 (SDA)_: Default I2C data line.
  * _GPIO22 (SCL)_: Default I2C clock line.
  * _GPIO34 - GPIO39_: Input-only pins used for ADC input.
  For detailed pin configuration and alternate functions, refer to the ESP32 Pinout Diagram below.

  ![download](https://github.com/user-attachments/assets/c09c4058-6b13-49f0-84d1-c94f5061dc95)

### Relay
A relay is an electrically operated switch that allows you to control a high-power device (like a motor or light) with a low-power signal (like the one from the ESP32).

* __Working Principle__: When the control signal is applied to the relay, it closes or opens its contacts, allowing or interrupting the current flow to the connected device.
* __Usage__: Used to control AC or high-power DC devices with the ESP32's 3.3V GPIO pins.

### LDR (Light Dependent Resistor)
An LDR, or Light Dependent Resistor, is an analog sensor whose resistance decreases with increasing light intensity.

* __Working Principle__: The resistance of the LDR varies inversely with light intensity. In bright light, the resistance is low, and in darkness, it’s high.
* __Usage__: It’s used to measure ambient light levels, and the analog signal can be read by the ESP32’s ADC to determine light intensity.

### PIR Sensor (Passive Infrared Sensor)
The PIR sensor detects infrared radiation, typically emitted by humans or animals, and is used to sense motion.

* __Working Principle__: The sensor detects changes in infrared radiation levels caused by moving objects and outputs a digital signal to the ESP32.
* __Usage__: It’s used in security systems, lighting controls, and other applications where motion detection is needed.

## Circuit Diagram

<img width="959" alt="image" src="https://github.com/user-attachments/assets/438be036-d750-4c0b-a4c1-ab2c276507e1">

## Wiring Instructions

1. **LDR Connection:**
   - Connect one end of the LDR to the 3.3V pin on the ESP32.
   - Connect the other end of the LDR to a point on the breadboard.
   - Connect a 10k ohm resistor from this point to GND.
   - Connect a wire from the point between the LDR and resistor to GPIO 34 on the ESP32.

2. **PIR Sensor Connection:**
   - Connect the VCC of the PIR sensor to the 3.3V pin on the ESP32.
   - Connect the GND of the PIR sensor to the GND pin on the ESP32.
   - Connect the output pin of the PIR sensor to GPIO 26 on the ESP32.

3. **Relay Module Connection:**
   - Ensure your relay module is compatible with 3.3V logic levels.
   - Connect the VCC of the relay module to the 3.3V pin on the ESP32.
   - Connect the GND of the relay module to the GND pin on the ESP32.
   - Connect the IN pin of the relay module to GPIO 25 on the ESP32.
   - Connect the relay's common terminal (COM) to one side of the load circuit.
   - Connect the normally open (NO) terminal to the other side of the load circuit.

## Connection
1. Open Wowki and create a new ESP32 project.
2. Add the ESP32 microcontroller to your workspace.
3. For the LDR and PIR sensor:
   * Connect the LDR, PIR sensor, and relay to the ESP32 according to the pinout and wiring diagram.
4. Ensure all connections are secure.

__LDR, PIR, and Relay:__

[Wokwi Link](https://wokwi.com/projects/405859383840502785)

## Code Example

```cpp
const int ldrPin = 34;        // LDR connected to GPIO 34
const int pirPin = 26;        // PIR sensor connected to GPIO 26
const int relayPin = 25;      // Relay connected to GPIO 25

void setup() {
  Serial.begin(115200);
  pinMode(pirPin, INPUT);
  pinMode(relayPin, OUTPUT);
  digitalWrite(relayPin, LOW);  // Ensure the relay is off initially
}

void loop() {
  int ldrValue = analogRead(ldrPin);
  int pirValue = digitalRead(pirPin);

  Serial.print("LDR Value: ");
  Serial.print(ldrValue);
  Serial.print(" | PIR Value: ");
  Serial.println(pirValue);

  // Example condition: turn on relay if it's dark and motion is detected
  if (ldrValue < 1000 && pirValue == HIGH) {
    digitalWrite(relayPin, HIGH);
  } else {
    digitalWrite(relayPin, LOW);
  }

  delay(100);  // Small delay to avoid flooding the serial monitor
}
```

## How It Works

- **LDR**: Measures the ambient light level. The ESP32 reads the analog signal and determines whether it is dark based on a threshold.
- **PIR Sensor**: Detects motion. When motion is detected, the PIR sensor sends a HIGH signal to the ESP32.
- **Relay**: The relay is controlled by the ESP32. When it's dark and motion is detected, the ESP32 activates the relay to turn on a connected device (e.g., a light).

## Safety Precautions

- **High Voltage Warning**: If you're using the relay to control AC loads, ensure you handle the connections carefully and follow all safety protocols to avoid electrical hazards.
- **Component Compatibility**: Ensure all components operate at the correct voltage levels to avoid damaging the ESP32 or other components.


