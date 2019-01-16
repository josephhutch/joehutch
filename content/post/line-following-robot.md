---
title: "PID Control for Line Following Robot"
date: 2018-12-21T11:30:19-05:00
description: "Line following is easy to accomplish but hard to master. PID Control, while more challenging to implement and tune, provides effective smooth tracking and quick response."
categories: ["Robotics"]
featuredImage: "img/lineBot.jpg"
featuredImageDescription: "A line following robot comprised of a Lego EV3 and hand-soldered sensor circuits"
dropCap: true
displayInMenu: false
displayInList: true
draft: true
---



## PID Based Line Following

Following a black line on a white background seems like a simple enough task, but achieving fast, smooth tracking while handling sharp turns requires a non-trivial algorithm. Fortunately, PID control is straight forward to implement and provides great results if properly tuned.

The idea behind PID control is simple: the difference between what you want, the set-point, and what you observe, the process value, determines how you react. The reaction, or how you change the control variable, can be tuned with three constants: {{< raw >}} \( K_p \) {{< /raw >}}, {{< raw >}} \( K_i \) {{< /raw >}}, and {{< raw >}} \( K_d \) {{< /raw >}}.

{{< raw >}}
\[u(t) = K_p e(t) + K_i \int_{0}^{t} e(\tau) d\tau + K_d \frac{de(t)}{dt} \]
{{< /raw >}}

The difficulty with PID control comes in tuning these constants. Manual tuning and using the Ziegler-Nichols method both proved unsuccessful in our attempts. We were able to get a successful combination of constants by increasing {{< raw >}} \( K_p \) {{< /raw >}} until the robot started to heavily oscilate, then increase {{< raw >}} \( K_d \) {{< /raw >}} to dampen the osciallations, and repeat until the robot could handle sharp turns with smooth execution.

It is important to know that the derivative term can amplify the effects of noise. Since we had a good amount of noise in our measurements and the noise frequency was much higher than the dynamics of our application, low pass filtering our measurements was crucial. Using a moving average filter removed unwanted noise while maintaining the integrity of the signal. This allowed the derivative term to improve our robot performance instead of throwing our robot off course.

## Robot Design

For the robot to detect the full range of possible headings (from “far left of the line” to “far right of the line”), three reflective light sensors were positioned approximately 2.8 cm above the ground and distributed evenly across the width of the line. Sensors exceedingly close the ground will be very sensitive to light intensity for a small circular area directly under the sensing element; sensors placed higher up will respond to a larger area under the sensing element at the expense of being less sensitive overall to color changes, owing to the increase in ambient light. The height of 2.8 cm provided the best balance of coverage area and sensitivity. In order to provide the robot the ability to handle sharp curves, the sensors were placed as close as possible to the axis of rotation. The reflective light sensor used is the EE-SF5B. Below is the schematic showing how the sensor is interfaced with the EV3 using a modular 6-pin connector cable.

{{<smallimg src="/img/reflectiveOpticalSensorSchematic.png" alt="Schematic for reflective light sensor">}}

An ultrasonic sensor is mounted above the color sensors pointing toward the forward direction of travel. The sensor helps the robot with platooning and avoiding obstacles. The ultrasonic sensor used is the SRF04. To have the sensor sample continuously, the SRF04's trigger is connected to a NE555 timer IC. Also, the PWM output from the SRF04 is put through an RC low pass filter so that the EV3 can sample it. The ultrasonic circuit is shown below.

{{<smallimg src="/img/ultrasonicSensorSchematic.png" alt="Schematic for the ultrasonic sensor">}}