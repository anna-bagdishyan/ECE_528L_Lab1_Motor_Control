# ECE 528/L - Robotics and Embedded Systems with Lab
**CSU Northridge**

**Department of Electrical and Computer Engineering**

## Motor Control Lab
The Motor Control lab interfaces with the following:

* User LEDs of the TI MSP432 LaunchPad
* Pololu Gearmotor with Encoder - [Product Link](https://www.pololu.com/product/3675)
* Left Bumper Switches for TI-RSLK MAX - [Product Link](https://www.pololu.com/product/3673)
* Right Bumper Switches for TI-RSLK MAX - [Product Link](https://www.pololu.com/product/3674)
* HS-485HB Servo-Stock Rotation - [Product Link](https://www.servocity.com/hs-485hb-servo/)

## Overview
This lab introduces PWM (Pulse Width Modulation) and edge-triggered interrupts using the TI MSP432 Launchpad and the TI-RSLK chassis. Timer_A2 was used to generate two 50 Hz PWM signals to control two HS-485HB servos, and the signals were verified on an oscilloscope by measuring the pulse widths at the 0 and 180 positions. Timer_A0 was then configured to generate PWM signals that drive the DC motors through the TI DRV8838 motors drivers, and motors functions were implemented to move the robot forward, backward, left, and right.

The bumper switches were configured to generate falling-edge interrupts on Port 4,

## Components Used
| Components Used | Quantity    | Manufacturer 
| -------------   | ----------- | -----------
| MSP432 Launchpad  | 1 | Texas Instruments
| USB-A to Micro-USB Cable  | 1 | N/A
| TI-RSLK MAX Chassis   | 1 | Pololu
| Bumper Switches  | 2 | Pololu
| HS-485HB Servo  | 1 | ServoCity
| Oscilloscope  | 1 | N/A
| Oscilloscope Probes  | 2 | N/A

## Analysis and Results
### Procedure
### Tasks 1, 2, and 3
The Bumper_Switches_Init() function was modified to configure P4.7, P4.6, P4.5, P4.3, P4.2, and P4.0 as GPIO inputs with pull-up resistors. The bumper switches were tested through the serial terminal. Pressing a switch generated an interrupt and displayed the corresponding 8-bit switch state. Bumper_Switches_Handler() was also modified to set P8.7 high when a bumper switch is pressed.

Figure # - Serial Terminal Output

The BMP0 signal at P4.0 was obsered on CH1 and P8.7 was observed on CH2. When BMP0 was pressed, switch bouncing occurred, and P8.7 responded to the falling edge interrupt.

Figure # - Oscilloscope photo of P4.0 and P8.7

### Task 4
### Task 5
### Task 6
### Task 7

## Known Issues or Limitations  

## Author Contribution  
Anna  

Mario  

## References
ECE 528/L Lab 1 - Motor Control.
