# NucleoCare-Disease Detection using STM32 Nucleo-F411RE micro controller

**NucleoCare** is a standalone embedded system for early disease detection, built using the **STM32 Nucleo-F411RE** microcontroller. It gathers physiological data from sensors, processes it using an on-board TinyML model, and predicts potential diseases — all without needing an external computer or cloud support.


## Problem Statement

Design and develop a **self-contained medical diagnostic device** that:
- Collects real-time physiological data using biomedical sensors
- Applies on-device inference using a TinyML model
- Predicts potential disease states with no cloud dependency


## Hardware Components

| Component         | Description                                      |
|------------------|--------------------------------------------------|
| **STM32 Nucleo-F411RE** | Main MCU platform with ARM Cortex-M4          |
| **MAX30102**      | Optical sensor for heart rate and SpO₂          |
| **MLX90614**      | Infrared temperature sensor for body temp       |


## ML Model & Tools

| Tool / Platform     | Purpose                                      |
|---------------------|----------------------------------------------|
| **STM32CubeIDE**     | Firmware development & peripheral setup      |
| **NanoEdge AI Studio** | TinyML model generation, training, and export |
| **STM32Cube.AI**     | Optional for further AI model integration    |


## System Architecture

The architecture of **NucleoCare** is designed for real-time biomedical signal processing and disease prediction, all running on the STM32 Nucleo-F411RE microcontroller without external computation or cloud support.

### Data Flow Pipeline

```text
[Sensors: MAX30102 + MLX90614]
           ↓
 [STM32 Nucleo-F411RE Microcontroller]
           ↓
[Signal Conditioning & Preprocessing]
           ↓
   [NanoEdge AI Inference Engine]
           ↓
   [Predicted Health Condition]
```

### 1. Sensor Data Acquisition

- **MAX30102 (Heart Rate, SpO₂ Sensor):**
  - Uses integrated LEDs and a photodetector to measure pulse oximetry and heart rate.
  - Communicates via **I²C protocol**.
  - Sampled periodically (e.g., every 100–500 ms) by the MCU.

- **MLX90614 (Body Temperature Sensor):**
  - Non-contact infrared sensor that provides accurate body temperature.
  - Communicates via **I²C protocol**.
  - Low-latency readings (~0.1s), directly processed by MCU.

### 2. Preprocessing on MCU

- Raw sensor values are cleaned and normalized using embedded signal processing routines.
- Preprocessing may include:
  - Rolling average or median filtering (to reduce noise)
  - Feature extraction (e.g., max HR, HRV, SpO₂ dips)
  - Temperature normalization or range encoding

### 3. TinyML Inference Engine (NanoEdge AI)

- A **classification model** generated using **NanoEdge AI Studio** is embedded into the firmware.
- Model is trained on labeled data such as:
  - `Healthy`
  - `Fever`
  - `Hypoxia`
- The exported **C library** from NanoEdge is compiled into the STM32 project.
- At runtime, the model receives the preprocessed features and performs **inference** entirely on the MCU.

### 4. Output Generation

- Based on inference results, the system displays or transmits the predicted condition:
  - Serial Output via UART
  - Optional: OLED Display / LED indicators
- Prediction latency is minimal (typically <100 ms), enabling real-time monitoring.


## Deployment Steps

1. **Sensor Integration**  
   - Configure I²C/UART for MAX30102 and MLX90614 using **STM32CubeMX** or directly in **CubeIDE**.
   - Read real-time heart rate, SpO₂, and temperature values.

2. **Model Creation with NanoEdge AI Studio**  
   - Collect sample data from sensors in real-world conditions.
   - Use **NanoEdge AI Studio** to generate a classification model.
   - Train the model on your labeled dataset (e.g., "Healthy", "Fever", "Low SpO₂").

3. **Model Deployment**  
   - Export C library from NanoEdge AI Studio.
   - Integrate the library in the STM32CubeIDE project.
   - Compile and flash onto the Nucleo board.

4. **Testing & Inference**  
   - Connect via serial monitor or OLED (optional) for output display.
   - Model runs locally and predicts disease state in real time.
  

## Output

- **Input**: Heart rate, SpO₂ level, Body temperature  
- **Output**: Predicted health status (e.g., Normal, Fever, Hypoxia)


## Future Improvements
- Add more sensors (ECG, GSR)
- Deploy neural network instead of anomaly/classifier models
- Support for edge dashboard or BLE-based alerts


## License
This project is licensed under the MIT License.



