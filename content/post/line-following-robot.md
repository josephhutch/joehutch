---
title: "PID Control for Line Following Robot"
date: 2018-12-21T11:30:19-05:00
description: "Line following is easy to accomplish but hard to master. PID Control, while more challenging to implement and tune, provides effective smooth tracking and quick response."
categories: ["Robotics"]
featuredImage: "/img/lineBot.jpg"
dropCap: true
displayInMenu: false
displayInList: true
draft: true
---

Line following offers a great introduction to robotics. A crude solution, such as bang-bang control, is easy to implement but provides much room for improvement. PID Control provides smooth tracking and quick response.

## Robot Design

For the robot to detect the full range of possible UV headings (from “far left of the line” all the way to “far right of the line”), three reflective light sensors were positioned approximately 2.8 cm above the ground and distributed evenly across the width of the line. Sensors exceedingly close the ground will be very sensitive to light intensity for a small circular area directly under the sensing element; sensors placed higher up will respond to a larger area under the sensing element at the expense of being less sensitive overall to color changes, owing to the increase in ambient light. The height of 2.8 cm provided the best balance of coverage area and sensitivity. In order to provide the UV the ability to handle sharp curves, the sensors were placed as close as possible to the axis of rotation. The reflective light sensor used is the EE-SF5B. Below is the schematic showing how the sensor is interfaced with the EV3 using a modular 6-pin connector cable.

![Schematic for reflective light sensor](/img/reflectiveOpticalSensorSchematic.png)

An ultrasonic sensor is mounted above the color sensors pointing toward the forward direction of travel. The ultrasonic sensor helps the robot with platooning and avoiding obstacles. The ultrasonic sensor used is the SRF04. To have the sensor sample continuously, the SRF04's trigger is connected to a timer. Also, the PWM output from the SRF04 is put through an RC filter so that the EV3 can sample it. The ultrasonic circuit is shown below.

![Schematic for the ultrasonic sensor](/img/ultrasonicSensorSchematic.png)



## PID Based Line Following

## PID Based Platooning

