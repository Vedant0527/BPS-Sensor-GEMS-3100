# BPS (Brake Pressure Sensor) Project

This project uses the **GEMS 3100 (0100S)** pressure sensor along with the **STM32 Nucleo-F446RE** development board to measure brake fluid pressure in automotive brake lines.

---

## 🛠️ Hardware Requirements

### Components
- **STM32 Nucleo-F446RE** (ARM Cortex-M4 board)
- **GEMS-3100 (0100S)** Pressure Sensor
- **12V Battery** – for powering the sensor
- **Connecting Wires** – for STM32 ↔ GEMS3100 ↔ Battery connections

---

## 🔌 Hardware Setup

### STM32 ↔ GEMS-3100 Sensor Wiring

| GEMS-3100 Pin     | Connected To         |
|------------------|----------------------|
| Vout (Pressure)  | PA0 (STM32 Analog In) |
| +IN (Power)      | 12V Battery           |
| Vout (Temp)      | Not Connected         |
| 0V (Ground)      | GND (Battery & STM32) |

> **Note**: Ensure common ground. Confirm voltage level compatibility — the sensor uses 12V.

---

## 🧰 Development Tools

| Tool              | Purpose                                           |
|------------------|---------------------------------------------------|
| STM32CubeIDE      | Main IDE for coding, compiling, flashing, debugging |
| STM32CubeMX       | (Optional) Configure STM32 peripherals with GUI    |
| Serial Terminal   | (Optional) UART communication (e.g., PuTTY, Tera Term) |

---

## 🧮 Pressure Calculation Formula

The STM32 reads analog voltage from the GEMS 3100 and converts it to pressure using the following logic:

- **Sensor Voltage Range**: 0.5V to 4.5V
- **STM32 ADC**: 12-bit resolution (0–4095)
- **Reference Voltage**: 3.3V

```c
// Convert ADC reading to pressure (in bar)
float Vol = (readValue / 4096.0) * 3.3;   // Convert ADC to voltage
float Voltage = Vol - 0.5;                // Remove offset
float Pressure = (Voltage / (3.3 / 4)) * 100; // Pressure calculation
