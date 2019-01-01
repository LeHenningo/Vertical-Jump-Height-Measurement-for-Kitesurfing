# Vertical-Jump-Height-Measurement-for-Kitesurfing
This project is about measuring the height of vertical jumps (in Kitesurfing), using a Accelerometer, Gyrometer, Magnetometer, (Barometer). To build a prototype the code will run on an Arduino Nano, which is connected to the sensors.

1. Introduction

Looking at the market for Jump Measurement Devices in Kitesurfing, you find that there are only two devices, which are used in Kitesurfing ("Duotone PIQ" and "WOO"). Both devices are very expensive (~200$). Moreover these devices are often critized, to measure unrealistic jump-heights. This project aims for a more affordable and even more precise alternative to the exisiting products. 

2. Concept
The following describes the concept for a prototyp device.
 
2.1 Hardware

The Prototyp-Hardware consists of an Arduino Nano which is connected to the used Sensors. Therefore the code in this Project is written in C. All used Sensors and the Arduino Nano use the I2C-Protocol for communication, which makes coding more easy. 

Used Sensors:

ACC/GYRO/MAG:
There a different ways to measure the height of a jump. The most common way might be the usage of a combination of Accelerometer/Gyrometer/Magnotemeter. Each single unit measures different movement vectors in 3 Axis. The combination is called a 9-Axis-Motion-Sensor. WOO claim on their website that they use this technology in their sensors. 
In the code the raw data of these 3 sensors have to be combined with quite complex maths, to generate a height-measurement. All three units measure the acceleration in different vectors. If you integrate the Acceleration (Meter per second per second) you are able to tell the distance/height the unit has moved. 

So far this sensor is used: MPU9250/6500

BARO:
Another interesting way to measure jump heigth is the usage of a barometer-sensor. It senses the change in atmospheric pressure. In stationary usage you can measure your height with an average error of 30cm. To convert the raw data of the sensor into a height measurement there is only very little math required. A problem could be, that the atmospheric pressure is influenced by high winds and high-/low-pressure weather. These conditions are typical circumstances in Kitesurfing. On the other hand, barometer-sensors are reliably used in UAVs which are also exposed to wind and changing airspeed. A workaround might be to constantly calibrate the barometer while no jump is registered. The ground zero would be constantly adjusted to changing conditions.

So far this sensor is used: GY-MS5803-14BA


Experiments have to show which sensor works best. Maybe the combination of both sensors could lead to a minimum of measurement-errors. 

[Wiring Diagram Arduino Nano & Sensors]

LCD-Screen:
An option would be a LCD-Screen which shows the reached height after or even while the jump.


Battery:
placeholder


2.2 Software 
The code has to run an Arduino Nano and therefore has to be written in C. 

Code for ACC/GYRO/MAG: 
For a correct measurement of height (Z-Axis) the sensor has to know in which directions it is tilted. Because the Kiteboard almost never lays perfectly flat on the water, while driving/jumping, the Z-Axis gets shifted and does not point straight in the air. This Shift has to be compensated. Before being able to register and measure a jump the sensor has to be able to constantly know it´s tilt. 

Knowing-it´s-tilt-Code: 

For measuring the tilt in every direction the raw data of the ACC- and GYRO- have to be combined. The combination of these values is quite complex maths. The different mathmatecial-methods are called filters. There two Filter-Options: 

1. Kalman-Filter: The Kalman-Filter is still quite complex to understand and use.
2. Dynamic-Filter: The Dynamic-Filter is much more simple than the Kalman-Filter. Moreover it has almost the same precision as the Kalman-Filter. Therefore the Dynamic-Filter will be used. 

Before the raw Data of the sensors can be applied to the Filter it has to be converted to the correct mathmatical unit (e.g. degrees per second). The time between the single measurements of the raw  data plays an important role here.



Height-Measurement-Code (Motion Sensor): 

If the sensor knows it´s tilt it should be easy to calculate a jump-height. For that you just have to convert the measured Acceleration (ACC-Sensor) in the Z-Axis (up and down) to a height, using a simple mathmatical formula. If the acceleration goes against 0 or becomes negative the peak of the jump was reached.



Height-Measurement-Code (BARO):

The height-measurement with the Barometer-sensor does not depend on knwoing-it´s-tilt. The Baro-Code converts the raw measured atmospheric pressure into Jump-Heights. The Math behind it would be not very complex.


Frame-Code:

The Frame-Code is a superior frame, which contains the mentioned functions. It determines if a jump is registered and if a height-measurement gets executed. It also contains the code for storing relevant values (jump-heights, jump-durations...) and maybe displaying values on the LCD-screen. 


 
