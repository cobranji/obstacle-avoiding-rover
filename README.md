# obstacle-avoiding-rover
Designed and assembled a two-wheeled robot capable of navigating indoor environments without human input. I integrated an HC-SR04 ultrasonic sensor with an Arduino to measure distances in real-time, writing a C++ control loop that translates sensor feedback into directional motor commands.

Obstacle-Avoiding Micro-Rover 🚗
Overview
This is a first-year undergraduate project (developed for the Interdisciplinary Design Project module at Aston University) demonstrating hardware-software integration and autonomous control logic.
The project features a custom-built, two-wheeled autonomous rover that navigates environments without human input. It uses an ultrasonic distance sensor to detect obstacles, and an Arduino processes this data in real-time to steer the motors.
To improve on standard hobbyist designs, the control firmware has been written as a non-blocking Finite State Machine (FSM). This ensures the processor never freezes and can continuously monitor the environment while executing movement maneuvers.
Hardware Components Used
Microcontroller: Arduino Uno (or Nano)
Motor Controller: L298N Dual H-Bridge Motor Driver
Sensor: HC-SR04 Ultrasonic Distance Sensor
Actuators: 2x standard 5V DC Gear Motors
Power: 9V Battery (for Arduino) + 4x AA Battery Pack / 7.4V Li-ion Pack (for Motors)
Chassis: Standard 2-wheel smart car chassis kit
How It Works
Instead of using linear, blocking code with the delay() function—which halts the processor completely—the rover utilizes an event-driven Finite State Machine (FSM) managed via non-blocking timing (millis()).
1. Non-Blocking Sensor Polling
The HC-SR04 ultrasonic sensor is polled every 60ms. To prevent the rover from locking up if an echo pulse is missed (which normally freezes an Arduino for up to 1 full second), the code implements a safe microsecond timeout limit on the pulseIn() function.
2. State Machine Transitions
The rover's logic flows dynamically through four distinct states:
DRIVE_FORWARD: The rover drives ahead while constantly checking the distance sensor. If an object is detected closer than 20cm, it stops the motors and switches states.
GEAR_PROTECT_PAUSE: The rover waits in place for 200ms. This allows physical momentum and back-EMF to dissipate, preventing stress on the plastic gearboxes and protecting the L298N motor driver from current spikes.
REVERSE: The motors run backward for 400ms to clear the immediate path.
TURN_RIGHT: The motors spin the chassis in-place to the right for 500ms before returning to forward movement or re-evaluating the path if still blocked.
How to Run This Project
Wire the Hardware:
Connect the HC-SR04 Trig pin to Arduino Pin 9, and Echo to Pin 10.
Connect the L298N direction inputs to Arduino Pins 4, 5, 6, and 7.
Connect the L298N PWM speed Enable pins to Arduino Pins 3 (ENA) and 11 (ENB). Make sure the jumpers on the L298N enable pins are removed first.
Setup the IDE: Open obstacle_rover.ino in the Arduino IDE.
Upload: Connect your Arduino via USB, select your board and port from the Tools menu, and hit Upload.
Test: Open the Serial Monitor (set to 9600 baud) to view the state transitions and live distance calculations. Unplug the USB, switch on the battery packs, and place the rover on the floor!
