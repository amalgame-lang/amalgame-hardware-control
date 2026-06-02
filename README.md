# amalgame-hardware-control

Pure-Amalgame **control & signal-processing** helpers — the bits every
robot, drone or 3D-printer build needs but rarely finds packaged: a PID
loop, an IMU complementary filter, smoothing, map/clamp. No hardware,
no dependency.

```sh
amc package add hardware-control
```

```amalgame
import Amalgame.Hardware.Control

// Heater PID
let pid = new Pid(2.0, 0.5, 0.1)
pid.SetSetpoint(60.0)
pid.SetLimits(0.0, 100.0)
let duty = pid.Update(measured, dt)        // % to send to a Pwm

// IMU angle (pairs with amalgame-hardware-sensor MPU6050)
let filt = new ComplementaryFilter(0.98)
let angle = filt.Update(gyroDegPerSec, accelAngleDeg, dt)
```

| Class | API |
|---|---|
| `Pid(kp,ki,kd)` | `SetSetpoint`, `SetGains`, `SetLimits`, `Update(measured, dt)`, `Reset` |
| `ComplementaryFilter(alpha)` | `Update(gyroRate, accAngle, dt)`, `Angle`, `Reset` |
| `MovingAverage(size)` | `Add(v)` → avg, `Value()` |
| `Range` (static) | `Map(x,inLo,inHi,outLo,outHi)`, `Clamp(x,lo,hi)` |

Requires amc ≥ 0.8.72. Apache-2.0.
