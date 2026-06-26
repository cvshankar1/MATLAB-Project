# MATLAB-Project

# Manipulator Kinematics, Trajectory Planning & Joint Control
**Advanced Robotics — MATLAB project**

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
