# MATLAB-Project

# Manipulator Kinematics, Trajectory Planning & Joint Control
**Advanced Robotics Assignment SS26 — MATLAB project**

## How to run

1. Open MATLAB (R2021a or newer) with the **Robotics System Toolbox** installed.
2. Open `main_manipulator_pipeline.m`.
3. Click **Run** (or press F5). No other files or setup are required — the
   script is self-contained and uses only the built-in UR5 model.
4. Six figures will appear (robot model, FK/IK use the command window,
   trajectory plots, desired-vs-actual plots, tracking-error plot, and a
   live animation), and all relevant numeric evidence is printed to the
   Command Window as the script runs.


```matlab
robot = loadrobot('kinovaGen3', 'DataFormat', 'column', 'Gravity', [0 0 -9.81]);
```
and set `eeName` to that robot's end-effector body name (e.g.
`'EndEffector_Link'` for the Kinova Gen3). The rest of the script is
robot-agnostic and needs no other changes.

## What the script does, mapped to the grading rubric

| Rubric item | Points | Where in the script |
|---|---|---|
| Robot model + FK | 15 | Sections **R1** and **R2** — loads UR5, prints joint count/names/end-effector, computes FK for two configurations |
| IK solver + reachability | 15 | Section **R3** — solves IK for two Cartesian targets built from reachable FK poses, prints solver status |
| Path / waypoint definition | 10 | Section **R4** — builds `qHome → qTarget1 → qTarget2 → qHome` |
| Trajectory planning + plots | 20 | Section **R5** — `trapveltraj`, plots of q, qd, qdd for all joints |
| Joint control implementation | 25 | Section **R6** — PD controller, desired-vs-actual plot, tracking-error plot |
| Simulation animation + interpretation | 10 | Final section — animates the robot using `qActual` (controlled motion, not the raw plan) |
| Reflection sheet quality | 5 | Not part of the code — see notes below |

## Notes for your reflection sheet

The script prints everything you need to answer the reflection-sheet
questions directly to the Command Window:
- Robot name, joint count, joint names, end-effector body (R1)
- Joint values and end-effector positions for both FK configurations (R2)
- IK target poses, solver status, and resulting joint solutions (R3)
- The full waypoint sequence (R4)
- Trajectory method and timing (R5)
- Controller type, gains, and per-joint RMS/max tracking error (R6)



## Tuning the controller

The PD gains are set near the top of the R6 section:
```matlab
Kp = 120 * ones(numJoints, 1);
Kd = 20  * ones(numJoints, 1);
```
Increasing `Kp` makes tracking faster but can introduce overshoot;
increasing `Kd` damps oscillation. Try a few combinations and discuss the
effect on the tracking-error plot in your reflection sheet — this is
explicitly asked for in the assignment ("explanation of gain effects").

If you want to demonstrate the PID alternative (mentioned as allowed in
the slides), add an integral term:
```matlab
integralError = integralError + e * dt;
u = Kp.*e + Ki.*integralError + Kd.*edot;
```
with `integralError` initialized to zero before the control loop.
