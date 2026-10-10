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

The bumper switches were configured to generate falling-edge interrupts on Port 4, and the effect of switch bouncing was observed on the oscilloscope. Finally, a collision-handling routine was implemented so that the robot stops, backs up, and turns right whenever a bumper switched pressed.

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
The Timer_A2 driver was used to generate two 50 Hz PWM signals on P5.6 and P5.7 to control two HS-485HB servos. Both pins were connected to the oscilloscope through a breadboard, with P5.6 on CH1 and P5.7 on CH2. In the main loop, the servos rotated to 0 with the RGB LED red, then to 180 with the RGB LED blue, switching every three seconds. At 0, the measured pulse width was µs, which is within the expected range of 540–580 µs. At 180°, the measured pulse width was µs, which is within the expected range of 2320–2360 µs.

![](https://github.com/anna-bagdishyan/ECE_528L_Lab1_Motor_Control/blob/main/Screenshots/ece528L_lab1_servo_0_degrees_group.JPG)  
Figure 1 - Oscilloscope photo of the servo (0 degrees)  

![](https://github.com/anna-bagdishyan/ECE_528L_Lab1_Motor_Control/blob/main/Screenshots/ece528L_lab1_servo_0_degrees_pulse_width_group.JPG)  
Figure 2 - Oscilloscope photo of the pulse width for the servo (0 degrees)  

![](https://github.com/anna-bagdishyan/ECE_528L_Lab1_Motor_Control/blob/main/Screenshots/ece528L_lab1_servo_180_degrees_group.JPG)  
Figure 3 - Oscilloscope photo of the servo (180 degrees)  

![](https://github.com/anna-bagdishyan/ECE_528L_Lab1_Motor_Control/blob/main/Screenshots/ece528L_lab1_servo_180_degrees_pulse_width_group.JPG)  
Figure 4 - Oscilloscope photo of the pulse width for the servo (180 degrees)  

### Tasks 1, 2, and 3
The Bumper_Switches_Init() function was modified to configure P4.7, P4.6, P4.5, P4.3, P4.2, and P4.0 as GPIO inputs with pull-up resistors. The bumper switches were tested through the serial terminal. Pressing a switch generated an interrupt and displayed the corresponding 8-bit switch state. Bumper_Switches_Handler() was also modified to set P8.7 high when a bumper switch is pressed.

![](https://github.com/anna-bagdishyan/ECE_528L_Lab1_Motor_Control/blob/main/Screenshots/ece528L_lab1_bumper_scope_shot_group.png)  
Figure 5 - Serial Terminal Output

The BMP0 signal at P4.0 was observed on CH1 and P8.7 was observed on CH2. When BMP0 was pressed, switch bouncing occurred, and P8.7 responded to the falling edge interrupt.

![](https://github.com/anna-bagdishyan/ECE_528L_Lab1_Motor_Control/blob/main/Screenshots/ece528L_lab1_bumper_terminal_output_group.JPG)  
Figure 6 - Oscilloscope photo of P4.0 and P8.7

### Task 4
The Timer_0A_PWM_Init() function was implemented in Timer_0A_PWM.c to generate the PWM signals for the DC motors, using Timer_A2_PWM_Init() as a reference. P2.6 and P2.7 were configured as PWM outputs for the right and left motors. CCR[3] and CCR[4] were set to Toggle/Reset mode and assigned the duty cycle values, and the timer was set to use SMCLK divided by 8 in Up/Down mode.

### Task 5
The Motor_Init(), Motor_Left(), and Motor_Right() functions were implemented in Motor.c, using Motor_Forward() and Motor_Backward() as references. Motor_Init() sets P5.4, P5.5, P3.6, and P3.7 as outputs, and sets up Timer_A0 with a 20ms period. Motor_Left() sets the left motor backward and the right motor forward so the robot turns left in place, while Motor_Right() does the opposite. Both functions update the duty cycles and set P3.6 and P3.7 high to enable the motors.

### Task 6
Drive_Pattern_1() was used to test the PWM and motor drivers. The function call was uncommented in the main loop, and the program was rebuilt and flashed. After testing, the Drive_Pattern_1() call was commented out.

### Task 7
The Handle_Collision() function was implemented in main.c to control how the robot reacts after a collision. When called, the robot stops for two seconds, moves backward for two seconds at 30% duty cycle, stops for one second, turns right for four seconds at 10% duty cycle, and stops for two more seconds. The collision_detected flag is then cleared to 0.

Bumper_Switches_Handler() was updated to set collision_detected to 1 when a bumper switch is pressed and no collision has already been detected. In the main loop, the robot moves forward until collision_detected is set, then calls Handle_Collision(). The program was rebuilt and flashed, and the robot was placed in the track. Each time the robot hit a wall, it backed up, turned right, and continued moving forward.

## Known Issues or Limitations  
There are no known current issues or limitations.  

## Author Contribution  
Anna  
- Wrote Tasks 1, 2, 3, and 6 in "Analysis and Results" on GitHub  
- Tasks 1, 2, 3, 4, and 5  

Mario  
- Wrote Tasks 4, 5, and 7 in "Analysis and Results" on GitHub  
- Tasks 6 and 7
- Wrote "Overview" on GitHub
## References
ECE 528/L Lab 1 - Motor Control.
