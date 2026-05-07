STM32 Ultrasonic Robot Controller with FSM and Low Power Mode

A finite state machine (FSM)-based embedded system developed on STM32 for obstacle-aware robot motion control using an HC-SR04 ultrasonic sensor, timer input capture, PWM motor control, and RTC-triggered low-power sleep management.

Project Overview

This project demonstrates the implementation of:

Finite State Machines (FSMs)
Non-blocking ultrasonic distance measurement
Timer Input Capture using interrupts
PWM-based motor speed control
RTC wake-up timer interrupts
Low-power sleep mode operation
Event-driven embedded systems design

The system periodically:

Enters low-power sleep mode
Wakes up using the RTC wake-up timer
Measures obstacle distance using the HC-SR04 ultrasonic sensor
Decides whether to move or stop based on the measured distance
Returns to sleep mode
System Architecture

The project uses two interacting finite state machines:

1. Main Control FSM (UC FSM)

Controls the robot behavior.

States
SLEEP
MEASURE
MOVE
STOP
2. Ultrasonic FSM (US FSM)

Handles non-blocking ultrasonic sensor measurement.

States
IDLE
TRIGGER_LOW
TRIGGER_HIGH
WAIT_ECHO
MEASUREMENT
Hardware Used
STM32 Nucleo Board
HC-SR04 Ultrasonic Sensor
DC Motor Driver
DC Motor
UART Serial Monitor
Peripheral Usage
Peripheral	Purpose
TIM2	Input Capture for Echo pulse measurement
TIM3	PWM motor control
TIM6	Microsecond timing for trigger pulse
RTC	Wake-up timer interrupt
UART2	Debugging and serial output
Key Embedded Systems Concepts Demonstrated
Finite State Machines (FSMs)

The project uses cooperative state-machine execution rather than blocking delays.

Interrupt-Driven Measurement

Echo pulse timing is captured using timer input capture interrupts.

Event-Driven Architecture

Flags are used to communicate events between ISRs and FSM logic:

measurementReady
distanceReady
rtc_flag
Low Power Embedded Design

The MCU periodically enters sleep mode and wakes up using RTC interrupts.

Distance Measurement Principle

The HC-SR04 sensor works by:

Sending a 10µs trigger pulse
Waiting for the echo pulse
Measuring the echo pulse width
Converting pulse duration to distance

Distance calculation:

distance = time * 0.034f / 2.0f;

Where:

time is in microseconds
0.034 cm/µs is the speed of sound
Important Design Decisions
Why FSMs Were Used

FSMs allow:

Non-blocking execution
Better scalability
Event-driven behavior
Cleaner embedded architecture
Why Interrupts Were Used

Input capture interrupts provide accurate pulse timing without CPU polling.

Why Sleep Mode Was Used

Sleep mode reduces CPU activity while keeping critical peripherals operational.

Challenges Encountered
Ultrasonic Measurement Freezing

Initially, the ultrasonic FSM was only executing one state transition per function call. This was solved by repeatedly calling the FSM until measurement completion.

Incorrect Distance Limitation (~24 cm)

Using STOP mode affected timer timing behavior after wake-up. Replacing STOP mode with SLEEP mode preserved timer operation and restored accurate measurements.

Example Serial Output
I have gone to sleep
I have woken up
Distance : 63.42

I have gone to sleep
I have woken up
Distance : 12.31
Future Improvements
RTOS-based task scheduling
IMU integration
Autonomous obstacle avoidance
Adaptive motor speed control
Power optimization refinement
Multi-sensor fusion
Learning Outcomes

This project demonstrates understanding of:

STM32 HAL drivers
Timer Input Capture
PWM generation
Interrupt handling
Finite State Machines
Event-driven firmware
Embedded low-power modes
Cooperative multitasking
Author

Emmanuel Odongo

Embedded Systems and Intelligent Control Enthusiast
