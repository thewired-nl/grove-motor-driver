# Grove-compatible Motor Driver TB6612
![3D render of the PCB](/render.png)

Circuit based on the [Adafruit breakout board](https://www.adafruit.com/product/2448) with the same chip, but with a Grove connector per motor channel and screw terminals for ease of use.
Datasheet: [TB6612FNG](https://cdn-shop.adafruit.com/datasheets/TB6612FNG_datasheet_en_20121101.pdf)

| Specs | |
| --- | --- |
| Motor voltage | 4.5 - 13.5v |
| Max amperage (per motor) | 1.2A (3.2A peak) |

This motor driver features 2 outputs (A and B), which can be individually controlled through their respective Grove connector.
Suitable for basic motors, linear actuators and solenoids.

# Wiring
Connect an external power supply between 4.5 - 13.v volts to the top screw terminal. Make sure ground is on the left (GND). Motor(s) can be connected to the outputs for Motor A (MA1, MA2) and/or Motor B (MB1, MB2) screw terminals. You can connect your motor wires either way, this only influences which throttle values will be forward/backward.

## Software
For use with CircuitPython, the [Adafruit motor library](https://docs.circuitpython.org/projects/motor/en/latest/index.html#) works quite well:

```python
import time
import board
import pwmio
import adafruit_motor

# One Grove port is used per motor output, make sure they don't have pin overlap
# TODO: figure out best PWM frequency

# A1, A2 Grove port connected to ItsyBitsy Expander port D2
pwmA1 = pwmio.PWMOut(board.D2, duty_cycle=0, frequency=5000)
pwmA2 = pwmio.PWMOut(board.D3, duty_cycle=0, frequency=5000)

# B1, B2 Grove port connected to ItsyBitsy Expander port D4
pwmB1 = pwmio.PWMOut(board.D4, duty_cycle=0, frequency=5000)
pwmB2 = pwmio.PWMOut(board.D7, duty_cycle=0, frequency=5000)

motorA = adafruit_motor.motor.DCMotor(pwmA1, pwmA2)
motorB = adafruit_motor.motor.DCMotor(pwmB1, pwmB2)

while True:
  # throttle ranges from -1.0 (full speed reverse) to 1.0 (full speed forward)
  motorA.throttle = 1
  motorB.throttle = 0
  time.sleep(2)
  motorA.throttle = -0.5
  motorB.throttle = -0.5
  time.sleep(2)
  motorA.throttle = 0
  motorB.throttle = 1
  time.sleep(2)
```

## License
[Creative Commons BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/)