
# STM32 Smart Gym Assistant

基于 STM32 的智能健身房助手

An embedded smart gym assistant based on STM32, designed to collect motion-related data, display real-time measurements, monitor pressure thresholds, and provide basic safety alerts during exercise.

## 📖 Project Overview

本项目是一个基于 STM32 的智能健身房助手，旨在通过传感器采集运动相关数据，并结合 LCD 显示、按键交互和蜂鸣器提醒，实现基础的运动数据监测与安全辅助功能。

系统使用压力传感器采集压力数据，通过 ADXL345 加速度传感器获取运动相关数据，并在 LCD1602 上显示测量结果。用户可以通过独立按键调整压力阈值，当检测数据满足相应判断条件时，系统执行蜂鸣器控制逻辑。

项目主要围绕嵌入式系统开发、传感器数据采集、外设驱动、数据处理和人机交互展开。

## ✨ Features

### 1. Sensor Data Acquisition

- Read pressure sensor data through the ADC.
- Acquire three-axis acceleration data from the ADXL345 sensor.
- Process sensor readings for motion-related monitoring.
- Use a threshold-based approach for basic step counting.

### 2. LCD Data Display

- Display pressure readings on the LCD1602.
- Display the configured pressure threshold.
- Display the current step counter.
- Provide a simple local interface for monitoring system data.

### 3. Adjustable Pressure Threshold

- Set an initial pressure threshold of 60.
- Increase the threshold using a physical button.
- Decrease the threshold using another physical button.
- Compare the acquired pressure value with the configured threshold.

### 4. Buzzer Alert Logic

- Compare the measured pressure value against the configured threshold.
- Control the buzzer according to the comparison result.
- Provide a basic hardware alert mechanism for threshold monitoring.

> Note: The current implementation uses threshold comparison logic. It should not be interpreted as a clinically validated exercise safety system or a complete motion-correction algorithm.

### 5. UART Serial Communication

- Initialize UART1 and UART2 at 9600 baud.
- Output pressure readings, configured thresholds, and step counts through serial communication.
- Support basic debugging and observation of runtime data.

## 🛠️ Hardware Components

| Component | Purpose |
|---|---|
| STM32 microcontroller | Main control unit |
| Pressure sensor | Pressure data acquisition |
| ADXL345 accelerometer | Three-axis acceleration data acquisition |
| LCD1602 | Local data display |
| Buzzer | Threshold-related alerts |
| Independent buttons | Pressure threshold adjustment |
| UART interface | Serial data output |

The exact STM32 model, board configuration, and additional circuit details should be documented according to the actual hardware used in the project.

## 💻 Software and Technologies

- **Programming Language:** Embedded C
- **Development Environment:** Keil MDK / µVision
- **Microcontroller Platform:** STM32
- **Sensor Interface:** ADC and ADXL345 driver
- **Display:** LCD1602
- **Communication:** UART serial communication
- **Peripheral Control:** GPIO, ADC, timers, and delay functions

## ⚙️ System Workflow

1. Initialize the required peripherals, including ADC, LCD, UART, buttons, and LED.
2. Initialize the ADXL345 sensor.
3. Read pressure sensor data through the ADC.
4. Acquire acceleration data from the ADXL345.
5. Apply threshold-based logic to update the step counter.
6. Display pressure readings, threshold values, and step counts on the LCD.
7. Compare pressure readings against the configured threshold and execute the buzzer control logic.
8. Detect button inputs and update the pressure threshold.
9. Output runtime data through UART for debugging and observation.

## 🧠 Implementation Details

### Pressure Data Acquisition

The program reads the ADC value associated with the pressure sensor and converts it into a displayed pressure-related value.

```c
press = GET_ADC_PA0 / 40.95;
```

The conversion factor is based on the current implementation and should be calibrated against the actual sensor and circuit before interpreting the value as a physical pressure measurement.

### Step Counting

The current implementation reads the ADXL345 sensor data and assigns the X-axis reading to a variable used by the step-counting logic.

A state flag, `step_i`, records whether a movement cycle has started.

- When the reading exceeds the positive threshold and no cycle is active, the state changes to started.
- When the reading falls below the negative threshold and a cycle is active, the state resets and the step counter increases.

The current code uses thresholds of `+30` and `-30`.

This is a basic threshold-based demonstration. Accuracy may vary with sensor orientation, movement patterns, noise, and the relationship between raw sensor readings and physical angles.

### Pressure Threshold Adjustment

The default threshold is initialized as follows:

```c
int press_yz = 60;
```

Two buttons are used to increase or decrease the threshold. The program compares the current pressure-related reading with this value and controls the buzzer according to the result.

## 📱 Planned Features and Current Limitations

The original design also proposed the following features:

- QR-code-based device login.
- Automatic synchronization of historical exercise data.
- A PC-side application for generating exercise reports.
- Personalized training recommendations based on exercise data.

These features are **not fully implemented** in the current project.

The Bluetooth module can display synchronized sensor data in a mobile Bluetooth application, but reliable data transmission to the PC-side application has not been completed. The report interface has been designed, but real-time report generation and personalized recommendations are not operational.

They are therefore listed as planned or incomplete features rather than completed capabilities.

## 🚀 Future Improvements

Potential directions for further development include:

- Improve the stability and completeness of Bluetooth data transmission.
- Define a structured communication protocol between the STM32 device and the PC-side application.
- Add data storage for historical exercise records.
- Complete the integration between the device, mobile interface, and PC-side application.
- Improve motion detection with sensor calibration, filtering, and more robust algorithms.
- Develop exercise reports and personalized recommendations after the data pipeline is complete.

These items represent possible future work and are not claims about the current implementation.

## 📂 Project Structure

The project uses modular embedded C source files and peripheral-related headers. The following is an illustrative structure; the actual repository should follow the files included in the Keil project.

```text
STM32-Smart-Gym-Assistant/
├── README.md
├── [Keil project file]
├── [Application source files]
├── [Peripheral driver source files]
├── [Header files]
└── [Hardware documentation and project photos]
```

Relevant modules referenced in the current report include:

- `adc.h` — ADC-related functions
- `tim.h` — timer-related functions
- `usart.h` — serial communication
- `adx345.h` — ADXL345 sensor functions
- `LCD1602.h` — LCD display functions
- `key.h` — button handling
- `led.h` — LED initialization
- `delay.h` — delay functions
- `sys.h` — system-related definitions

Actual source file availability and naming may vary with the project directory.

## 🔧 Development Environment

To inspect or continue developing this project:

1. Install Keil MDK / µVision.
2. Open the actual Keil project file included in the repository.
3. Check the STM32 device configuration and required libraries.
4. Verify the hardware connections and peripheral configuration.
5. Build the project and resolve any environment-specific dependencies.
6. Connect the corresponding hardware to test sensor acquisition, LCD output, buttons, buzzer logic, and UART output.

The exact MCU model, clock configuration, and library version should be confirmed from the original project files.

## 📸 Project Demonstration

Project photos, hardware setup images, LCD output, and application screenshots can be added here.

Suggested evidence to include:

- Overall hardware setup
- STM32 board and sensor connections
- LCD data display
- Pressure threshold adjustment
- Buzzer response
- UART serial output
- Bluetooth application data display

Only include images and demonstrations that correspond to functions actually tested.

## 📚 Project Background

This project was developed as part of the course **Embedded Systems and Applications** at Baoding University in June 2025.

It provided practical experience with STM32 peripheral initialization, sensor interfacing, ADC data acquisition, LCD display control, button input handling, threshold-based logic, and UART debugging.

## 📄 License

No license has been specified yet. Unless a license is added to the repository, reuse and redistribution permissions are not explicitly granted.

---

**Project:** STM32 Smart Gym Assistant  
**Platform:** STM32  
**Language:** Embedded C  
**IDE:** Keil MDK / µVision
