# FPGA-Accelerometer-Interface

## Overview
This project aims to develop a system capable of displaying acceleration data from a gyroscope on a DE10-Lite board in real time. The primary goal is to measure acceleration along the X, Y, and Z axes at regular one-second intervals and display these values on the board’s 7-segment display.

To achieve this, the system relies on the Nios II processor, which facilitates the integration of both hardware and software functionalities. The accelerometer is interfaced using either the I2C or SPI communication protocols, ensuring precise data reading and writing.

A key step in this project is calibrating the sensor data. Calibration is performed using gravity to ensure optimal measurement accuracy, which is essential for reliable data display.

To enhance interactivity, a push button allows users to switch between different axis values, providing an intuitive and user-friendly experience. This project highlights the seamless integration of hardware and software in an embedded system solution.

## System Architecture

### Components
- **Nios II Processor**: Controls the system.
- **I2C Communication Module**: Interfaces with the ADXL345 accelerometer.
- **Bin_to_7seg Module**: Converts binary data for the 7-segment display.
- **Push Button**: Switches between X, Y, and Z axes.
- **Timer Module**: Synchronizes data reading at regular one-second intervals.

### Component Integration
In Platform Designer, several essential components were added:
- **Nios II Processor** for data processing.
- **JTAG UART Module** for communication with the host via UART.
- **OpenCores I2C Module** for I2C communication with the ADXL345 sensor.
- **PIO Modules** to control the 7-segment displays (SEG0 to SEG5) and buttons (KEY).
- **Timer Module** to generate periodic interrupts for synchronized data updates.

These components are interconnected through the Avalon bus, ensuring a fully functional system for sensor data management and display.

## Implementation

### I2C Initialization
The `I2C_init` function configures the I2C peripheral with a clock frequency of 100 kHz, ensuring stable communication. An I2C start command verifies the sensor’s presence at address `0x1D`, ensuring error-free read and write operations.

### UART Display
The X, Y, and Z axis registers (`X0 (0x32)`, `X1 (0x33)`, `Y0 (0x34)`, `Y1 (0x35)`, `Z0 (0x36)`, `Z1 (0x37)`) are read via I2C. The low (LSB) and high (MSB) byte values are combined to form 16-bit acceleration values for each axis. 

Data is displayed in hexadecimal format using `alt_printf`, providing real-time visualization for debugging and analysis.

### Calibration
Calibration was performed by adjusting the offset registers for the X, Y, and Z axes (`OffsetX (0x1E)`, `OffsetY (0x1F)`, `OffsetZ (0x20)`).
- Raw sensor values were analyzed through UART.
- Offset values were adjusted iteratively until stable, accurate readings were obtained.
- This process improved measurement reliability and precision.

### 7-Segment Display
- Raw sensor data is converted into signed values.
- Data is scaled using a factor of **3.9 mg** per unit.
- The `bin_to_7seg` module handles binary-to-display conversion.
- A dedicated segment displays a negative sign for negative values.

### Axis Display Switching with Button
An interactive method was implemented to switch between X, Y, and Z axis displays using a button-triggered interrupt. Each button press triggers an interrupt service routine that updates the displayed axis dynamically.

### Timer Usage
A hardware timer generates interrupts every second, ensuring automatic and regular data updates. At each interrupt:
- Sensor data is read via I2C.
- UART displays the updated values.
- 7-segment displays are refreshed with new values.

This one-second delay ensures an optimal balance between visualization needs and processor load efficiency.

## Conclusion
This project successfully implemented an embedded system capable of measuring and displaying real-time acceleration data from the ADXL345 sensor. By integrating:
- **Nios II processor and Avalon bus** for efficient data handling.
- **I2C communication** for sensor interfacing.
- **Calibration techniques** to ensure measurement accuracy.
- **7-segment display and UART output** for real-time visualization.
- **Timer and button-driven interrupts** for an interactive experience.

This project highlights the importance of a modular and integrated approach in embedded system design. The development of dedicated VHDL code and the use of Platform Designer tools helped overcome technical challenges, enhancing both hardware and software co-design skills.

