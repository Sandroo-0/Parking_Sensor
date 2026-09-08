# Parking_Sensor
An Arduino-based ultrasonic parking sensor that measures distance, displays it on an I2C LCD, and provides visual warnings using three LEDs.
# Ultrasonic Parking Sensor

An Arduino-based parking distance sensor that uses an **HC-SR04 ultrasonic sensor** to measure the distance between the sensor and an object.

The measured distance is displayed on a **16x2 I2C LCD**, while three LEDs provide a visual indication of the distance:

* **Green** — Safe distance
* **Yellow** — Caution
* **Red** — Object is close
* **Rapidly blinking red** — Stop / danger

The system becomes more urgent as the object gets closer.

## Features

* Real-time distance measurement using an HC-SR04
* Distance displayed in centimeters on a 16x2 I2C LCD
* Green, yellow, and red warning LEDs
* Increasing red LED blink rate as the object gets closer
* Detection when no echo is received
* Optional buzzer support
* Adjustable distance thresholds

## Components

* Arduino Uno
* HC-SR04 ultrasonic distance sensor
* 16x2 I2C LCD
* Green LED
* Yellow LED
* Red LED
* 3 × resistors for the LEDs
* Optional buzzer
* Breadboard
* Jumper wires

## Wiring

### HC-SR04

| HC-SR04 Pin | Arduino       |
| ----------- | ------------- |
| VCC         | 5V            |
| GND         | GND           |
| TRIG        | Digital Pin 8 |
| ECHO        | Digital Pin 7 |

### LCD 16x2 I2C

| LCD Pin | Arduino |
| ------- | ------- |
| VCC     | 5V      |
| GND     | GND     |
| SDA     | A4      |
| SCL     | A5      |

The LCD uses I2C address `0x27`.

### LEDs

| LED    | Arduino Pin   |
| ------ | ------------- |
| Green  | Digital Pin 2 |
| Yellow | Digital Pin 3 |
| Red    | Digital Pin 4 |

Each LED is connected with a suitable current-limiting resistor.

### Optional Buzzer

| Buzzer | Arduino        |
| ------ | -------------- |
| +      | Digital Pin 13 |
| -      | GND            |

The buzzer is disabled by default in the code.

## Distance Thresholds

The project uses four distance zones:

| Distance         | Status  | Indicator               |
| ---------------- | ------- | ----------------------- |
| More than 100 cm | SAFE    | Green LED               |
| 31–100 cm        | CAUTION | Yellow LED              |
| 11–30 cm         | CLOSE!  | Red LED blinking        |
| 10 cm or less    | STOP!!  | Red LED blinking faster |

These values can be changed in the Arduino code.

```cpp
const int SAFE_DISTANCE = 100;
const int CAUTION_DISTANCE = 30;
const int DANGER_DISTANCE = 10;
```

## How It Works

1. The HC-SR04 sends an ultrasonic pulse.
2. The sensor waits for the echo to return.
3. Arduino calculates the distance using the echo travel time.
4. The measured distance is displayed on the LCD.
5. Arduino determines the current distance zone.
6. The appropriate LED is activated.
7. When the object gets very close, the red LED blinks increasingly quickly to provide a stronger warning.

If the sensor does not receive an echo within the timeout period, the LCD displays `---` and the system returns to the SAFE state.

## Optional Buzzer

The program also supports an optional buzzer.

To enable it, change:

```cpp
#define USE_BUZZER false
```

to:

```cpp
#define USE_BUZZER true
```

When enabled, the buzzer provides increasingly frequent warnings as the object approaches.

When the buzzer is disabled, the red LED is used instead.

## Required Arduino Library

The project uses:

```cpp
#include <Wire.h>
#include <LiquidCrystal_I2C.h>
```

`Wire` is included with the Arduino IDE.

The **LiquidCrystal_I2C** library is required for the I2C LCD.

## Files

```text
Ultrasonic-Parking-Sensor/
├── README.md
├── Ultrasonic_Parking_Sensor.ino
├── wiring/
│   └── wiring-diagram.png
└── LICENSE
```

## Project Goal

The goal of this project is to demonstrate how an Arduino can combine **distance sensing, LCD output, and visual warnings** to create a simple parking-assistance system.

The project can be adapted for other applications where detecting the distance to nearby objects is useful.
