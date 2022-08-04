In this tutorial we will learn how to control a brushless motor using Arduino and ESC. In case you want more details how BLDC motors work, you can check the other article or watch the following video which contains explanation of the working principle of a brushless motor and how to control one using Arduino and ESC.![image](https://user-images.githubusercontent.com/109545803/182963372-fdd62acb-1190-4858-ad6f-3b39792ace1d.png)



In this case, the 1000KV means that, for example, if we supply the motor with 2S LiPo battery which has a voltage of 7.4 volts, the motor can achieve maximum RPM of 7.4 times 1000, or that’s 7400 RPM.

Brushless motors are power hungry and the most common method for powering them is using LiPo batteries. The “S” number of a LiPo battery indicates how many cells the battery has, and each cell has a voltage of 3.7V.
![image](https://user-images.githubusercontent.com/109545803/182963515-03c4b111-a20f-44e1-81a8-2a1f6e9eb347.png)

For this example, I will use 3S LiPo battery which has 3 cells and that’s 11.1V. So, I can expect my motor to reach maximum RPM of 11100.

Lastly, here’s a 30A ESC that I will use for this example and match with the motor requirements. On one side the ESC has three wires that control the three phases of the motor and on the other side it has two wires, VCC and GND, for powering.

![image](https://user-images.githubusercontent.com/109545803/182963819-b054e9c9-c7f6-4cba-9b22-14581a0f42b2.png)

There is also another set of three wires coming out of the ESC and that’s the signal line, +5V and ground. This feature of the ESC is called Battery Eliminator Circuit and as the name suggests it eliminates the need of separate battery for a microcontroller. With this, the ESC provides regulated 5V which can be used to power our Arduino.

We can notice here that this connection is actually the same as the one we see on Servo motors.

![image](https://user-images.githubusercontent.com/109545803/182963902-4ba91ed3-53b7-4331-84f0-8e7cd5bcb992.png)

So, controlling a brushless motor using ESC and Arduino is as simple as controlling servo using Arduino. ESCs use the same type of control signal as servo and that’s the standard 50Hz PWM signal.
![image](https://user-images.githubusercontent.com/109545803/182963998-1d951b5d-e36a-4be5-8fb1-99332c315388.png)

This very convenient, because for example, when building an RC plane, we usually need both servos and brushless motors and, in this way, we can control them easily with the same type of controller.

So, using the Arduino we just have to generate the 50Hz PWM signal and depending on pulses width or the high state duration which should vary from 1 millisecond to 2 milliseconds, the ESC will drive the motor from minimum to maximum RPM.
![image](https://user-images.githubusercontent.com/109545803/182964027-8a083a4a-13ad-4959-a2fe-eb78810a2402.png)

Here’s the circuit diagram for this example. In addition to the ESC we will just use a simple potentiometer for controlling the motor speed.

![image](https://user-images.githubusercontent.com/109545803/182964067-c9c31c47-e85c-48fd-a5ad-45b0691c474e.png)

The Arduino code is really simple with just few lines of code check the code here

Description: So, we need to define the Servo library, because with the servo library we can easily generate the 50Hz PWM signal, otherwise the PWM signals that the Arduino generates are at different frequencies. Then we need to create a servo object for the ESC control and define a variable for storing the analog input from the potentiometer. In the setup section, using the attach() function, we define to which Arduino pin is the control signal of the ESC connected and also define the minimum and maximum pulses width of the PWM signal in microseconds.

In the loop section, first we read the potentiometer, map its value from 0 to 1023 into value from 0 to 180. Then using the write() function we send the signal to the ESC, or generate the 50Hz PWM signal. The values from 0 to 180 correspond to the values from 1000 to 2000 microseconds defined in the setup section.

So, if we upload this code to our Arduino, and then power up everything using the battery, then we can control the speed of the brushless motor of zero to maximum using the potentiometer.

![image](https://user-images.githubusercontent.com/109545803/182964098-afa1a675-0072-4010-a72f-18dc9e4a9997.png)

However, there are few things that we should note here. When initially powering the motor, the signal value must be the same or lower than the minimum value of 1 millisecond. This is called arming of the ESC, and the motor makes a confirmation beeps so that we know that it’s properly armed. In case we have higher value when powering, which means we have a throttle up, the ESC won’t start the motor until we throttle down to the correct minimum value. This is very convenient in terms of safety, because the motor won’t start in case we have a throttle up when powering.

ESC Calibration
Lastly, let’s explain how ESC calibration works. Every ESC has its own high and low points, and they might slightly vary. For example, the low point might be 1.2 milliseconds and the high point might be 1.9 milliseconds. In such a case, our throttle won’t do anything in the first 20% until it reaches that low point value of 1.2 milliseconds.
