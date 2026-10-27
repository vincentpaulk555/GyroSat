# GyroSat

A 1-axis miniature spacecraft attitude-control testbed driven by a homemade control moment gyroscope, built from scratch to learn the physics, mechanics, and control code.

## What is a control moment gyroscope?

A control moment gyroscope (CMG) is an actuator that rotates a spacecraft **without expelling any propellant**. It works by storing angular momentum in a fast-spinning flywheel and then *tilting* that flywheel to redirect where its momentum points.

Angular momentum is conserved, so changing the direction of the flywheel's momentum forces an equal and opposite reaction on the vehicle it is mounted to. The governing relationship is Newton's rotational law:

```
tau = dH/dt
```

Torque equals the rate of change of angular momentum. For a CMG, the flywheel's spin momentum `h` has (nearly) constant magnitude but its *direction* changes as a gimbal rotates it. If the gimbal turns at rate `delta_dot`, the reaction torque on the platform is:

```
tau = h x delta_dot
```

So a small gimbal motion applied to a large stored momentum produces a large torque. This "torque amplification" is why real satellites (and the ISS) use CMGs instead of burning fuel to point themselves.

## How GyroSat works

GyroSat is a miniature **ADCS (Attitude Determination and Control System)**. A platform rotates freely about one axis on a low-friction stand, and a CMG mounted on it steers it:

- **Determine** — an IMU (MEMS gyro + accelerometer) measures the platform's angle and rotation rate.
- **Control** — an Arduino runs a PID controller that compares the measured angle to the target and decides how to move the gimbal.
- **Actuate** — the CMG gimbals its spinning flywheel, producing the reaction torque that turns the platform.

```
        +--------------------+
        |   Target angle     |
        +---------+----------+
                  |
                  v
   measured   +---+----+     gimbal cmd    +-----------+
   angle/rate |  PID   |  --------------->  |    CMG    |
      ^       |(Arduino)|                   | (flywheel |
      |       +--------+                    |  + gimbal)|
      |                                     +-----+-----+
      |                                           | reaction torque
      |                                           v
   +--+-----+                              +---------------+
   |  IMU   |  <-------------------------  |   Platform    |
   | sensor |         rotates              | (1-axis free) |
   +--------+                              +---------------+
```

The loop is closed: the CMG torques the platform, the IMU senses the new angle, and the PID keeps correcting until the platform reaches and holds the commanded attitude.

## Project goals

- **Slew** — command the platform to a target such as +45 degrees and get there cleanly.
- **Hold** — settle at the target and keep it steady with minimal drift or oscillation.
- **Reject disturbances** — recover the target attitude after being pushed by hand.

## Roadmap

- [~] **Phase 0 — Physics foundations** *(in progress)*: angular momentum, gyroscopic torque, rotational dynamics, and CMG limits (cos(delta) falloff, gimbal saturation, off-axis torque).
- [ ] **Phase 1 — Simulation (Python)**: free platform, then CMG dynamics, then PID control.
- [ ] **Phase 2 — CAD design and sizing**: flywheel, motor, gimbal, and low-friction test stand.
- [ ] **Phase 3 — Mechanical build (V1)**: physical rig with the gimbal moved by hand.
- [ ] **Phase 4 — Electronics**: Arduino + IMU wiring and bring-up.
- [ ] **Phase 5 — Closed-loop control on hardware**: run the controller on the rig and compare against the simulation (system identification).
- [ ] **Phase 6 — Advanced CMGs**: scissored CMG pair, multiple CMGs, and singularity avoidance.

## Repository structure

| Folder | Purpose |
| --- | --- |
| `docs/` | Physics notes, derivations, exercise answers, and design decisions. |
| `sim/` | Python simulation of the platform, CMG, and controller. |
| `cad/` | CAD exports and drawings. |
| `firmware/` | Arduino code for the controller. |
| `data/` | Telemetry logs captured from the real rig. |

## Planned tech

- **Simulation** — Python on macOS.
- **CAD** — Fusion 360 / Onshape.
- **Hardware** — Arduino + IMU (MEMS gyro + accelerometer). No hardware purchased yet.

## Learning approach

I'm building GyroSat from scratch to learn the physics, mechanics, and control code hands-on rather than to ship a product. I work through each phase in order, deriving the math and writing the code myself so the understanding sticks.
