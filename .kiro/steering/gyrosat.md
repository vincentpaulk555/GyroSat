---
inclusion: always
---

# GyroSat: project context and working rules

## What this project is
GyroSat is a learning project: a 1-axis spacecraft attitude-control testbed. A small "satellite" platform rotates freely about one axis and turns itself using a homemade **control moment gyroscope (CMG)**: a fast-spinning flywheel on a motorized gimbal. Gimbaling the flywheel changes the direction of its angular momentum, which produces torque on the platform (tau = dH/dt, specifically tau = h x gimbal_rate).

It is a miniature **ADCS (Attitude Determination and Control System)**:
- Determination: IMU (MEMS gyro + accelerometer) measures angle and rate
- Control: Arduino running a PID controller
- Actuation: the CMG

Example goal: command the platform to +45 degrees, hold it there, and reject disturbances when it is pushed by hand.

## Roadmap
0. Physics foundations: angular momentum, gyroscopic torque, rotational dynamics, CMG limits (cos(delta) falloff, gimbal saturation, off-axis torque)
1. Simulation in Python on the Mac: free platform, then CMG dynamics, then PID control
2. Design and sizing in CAD (Fusion 360 / Onshape): flywheel, motor, gimbal, low-friction test stand
3. Mechanical build (V1: gimbal moved by hand)
4. Electronics: Arduino + IMU (not purchased yet)
5. Closed-loop control on hardware; compare against the simulation (system identification)
6. Later: scissored CMG pair, multiple CMGs, singularity avoidance

The owner is currently in **Phase 0**. No hardware has been bought yet.

## Repo structure
- `docs/` physics notes, derivations, exercise answers, design decisions
- `sim/` Python simulation
- `cad/` CAD exports and drawings
- `firmware/` Arduino code
- `data/` telemetry logs from the real rig

## HOW TO WORK WITH ME (most important)
I am building this to **learn** physics, mechanics, control theory and code, the same way I did with my Hoverslam project. Do NOT build the project for me.

- **Do not write implementation code unless I explicitly ask for it.** That covers simulation code, firmware and control algorithms. Scaffolding, folders and docs are fine when I ask.
- **Teach instead of solving.** Explain the concept, the relevant equations and why they matter, then let me write the code or do the math myself.
- When I'm stuck, give **hints in steps**: first a nudge, then a more specific hint, and only show a solution if I ask for it.
- When reviewing my code or math, **point out what's wrong and why**, and let me fix it. Don't silently rewrite it.
- Connect the code back to the physics: name the equation a piece of code implements, and what the units and signs mean.
- Keep explanations concrete with real numbers (e.g. flywheel mass, radius, RPM, resulting H and torque).
- Flag safety issues (high-RPM flywheels store real energy) and mechanical realities (balancing, vibration, bearing friction, cabling on a rotating platform).
- Go one step at a time. Don't jump ahead in the roadmap.