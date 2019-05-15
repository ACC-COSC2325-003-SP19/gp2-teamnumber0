=====================================
Group Project 2: AVR Hardware Control
=====================================



Chosen Device: Generic HC-SRO4 Sonar Distance Sensor
====================================================

**Eri Midorikawa**

--------------
Basic Features
--------------

This ultrasonic sensor uses sonar to determine the distance to an object.

* 5V Power Supply
* Quiescent Current : <2mA
    - Quiescent current, or I\ :sub:`Q`\ , is the current drawn by the component in a no-load (meaning no current leaves the component to the output) and nonswitching (meaning no power switch to the component is on) but enabled condition
* Working Current: 15mA
* Effectual Angle: <15°
* Ranging Distance : 2cm – 400 cm or 1″ – 13ft
* Resolution : 0.3 cm
* Measuring Angle: 30 degree
* Trigger Input Pulse width: 10 μs (microseconds)
* Dimension: 45mm x 20mm x 15mm

------
Source
------

https://www.electroschematics.com/8902/hc-sr04-datasheet/

Controlling the Device
======================

------------------------
Input and Output Signals
------------------------

**Leslie Durkey**

The trigger pin serves as the transmitter and sends an output signal: a high-frequency sound. When the signal finds an object, it is reflected and the echo pin, or receiver, behaves as the input signal.

Device Demonstration
====================

**Shaun Peretz**

.. image:: https://media.giphy.com/media/kfWzLfRhE2R9cAmpQG/giphy.gif


.. image:: https://media.giphy.com/media/dxIhwKxYkY5RXHEQwe/giphy.gif



Project Code
============

**Bobby Galvan**

::

    // Define pins
    const int trigPin = 11;
    const int echoPin = 12;
    
    // Define variables
    long duration;
    int cmDistance, inDistance;
    
    void setup() {
      // Set input and output
      pinMode(trigPin, OUTPUT);
      pinMode(echoPin, INPUT);
      
      // Begin serial communication at a 9600 baud rate
      Serial.begin (9600);
    }
    
    void loop() {
      // Clear the trigPin for clean pulse
      digitalWrite(trigPin, LOW);
      delayMicroseconds(3);
    
      // Pulse trigPin for 10 microseconds
      digitalWrite(trigPin, HIGH);
      delayMicroseconds(10);
      digitalWrite(trigPin, LOW);
    
      // Read signal from sensor
      duration = pulseIn(echoPin, HIGH);
    
      // Calculate output in cm and inches:
      // Divide by 2 to account for the wave hitting the object and 
      // returning to the sensor, and multiplying by the speed
      // of sound (*1/21.9 for cm, 1/74 for inches)
      cmDistance = (duration/2)/21.9;
      inDistance = (duration/2)/74;
    
      // Print results
      Serial.print("Distance: ");
      Serial.print(inDistance);
      Serial.print(" in, ");
      Serial.print(cmDistance);
      Serial.print(" cm");
      Serial.println();
    }
