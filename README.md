# Synthax-Hackathon-Final
SYNTHAX HACKATHON- FUTURISTIC WALKING STICK FOR VISUALLY IMPAIRED
SYNTHAX HACKATHON- FUTURISTIC WALKING STICK FOR VISUALLY IMPAIRED

Problem Statement:
Traditional walking stick used by the visually impaired are cumbersome and difficult to use with efficiency.

Aim:
To replace walking sticks for the visually impaired with a modern and technological approach which is more efficient.

Hardware Stack:

Sl.	Material and Units
1	Microcontroller (ESP32)- 1
2	VL53L0X ToF LiDar- 1
3	Vibration Motors- 2
5	Jumper wires- 1
6	3D printed enclosure for parts- 1
7	Breadboard- 1
8	Power Bank & 9V Battery- 1
9	LED bulb- 1


Software Stack-

    Layer                        Technology 

1. Hardware Communication	                Wire.h	                          I2C communication between ESP32 

2. Sensor Driver	                      VL53L0X.h	                          Controls the ToF sensor; handles distance 
                                                                            measurement & timeouts

3. Core Logic	                          Arduino C/C++	                      Main loop, pulse timing, state management

4. Microcontroller	                    ESP32 Arduino Core	                Provides GPIO, timers, and millis() timing 
                                                                            functions

5. Feedback Control	                    Digital GPIO	                      ON/OFF vibration motor & LED 
    pulsing

Serial Debug Layer	                    Serial (UART)	                       Monitoring distance and errors
•	Code architecture is written in form of comments in the code


Setup Instructions:
1.	Take the prototype of the product
2.	Ensure that the ToF sensor and vibration motors are OUTSIDE the enclosure through the hole in the 
    enclosure.
3.	Secure the enclosure the outer side of your lower thigh using the velcro strap provided
4.	Secure the vibration motors to your thigh using tape
5.	Secure the ToF sensor to the front of your thigh using double sided tape. Ensure that the side of the sensor which says 
    VL53L0X, VIN, GND, SCL, SDA is facing outwards towards the obstacles and that this side is not obstructed by any tape or 
   material.
6. Connect the power bank using the provided USB cable to the ESP32 OR turn on the switch if a 9V battery is used.
7. Test the prototype of this project by walking towards a wall.


User Interface:
All electronics and components are stored in an enclosure with a Velcro strap which easily wraps around a person’s thigh.

Advantage Over Existing Technology:
This cap for the visually impaired offers a seamless and hands-free experience for them as opposed to the traditional walking stick. It is also more reliable than the stick and offers a greater range of 2m.

Division Of Work Between Team Members (NPS HSR-Grade 9)
1.	Aditya Vishwajit- Coding of ESP32 and software architecture
2.	Vedant Candadai- Coding of ESP32 and software architecture
3.	Agastya Girish- Project assembly

Disclosure (Use of AI tools and Libraries Used):
Model-ChatGPT 5
Prompt 1- ‘Generate ideas to make this project even better’ (Didn’t have the function where pulse intervals become less as obstacle comes closer)
Output- Introduce a functionality to reduce time intervals between pulses as theobject gets closer
NOTE- ChatGPT 5 did NOT generate the code for the logic of reduction in time interval between pulses and wasn’t used for any coding.
Prompt 2- ‘Explain what a tech stack is.’
Output- Explanation of tech stacks
Libraries Used-
1.	Wire.h- Used for I2C communication with the VL53L0X ToF sensor
2.	VL53L0X.h- Used for functions of the ToF sensor which calculate the distance between the sensor and the obstacle in front of it.

