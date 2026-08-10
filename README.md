# 🤖 Distributed Formation Control: Simulation Tutorial

This repository contains the MATLAB simulation for a distributed control architecture, designed to make a fleet of heterogeneous mobile robots move in a rigid triangular formation while tracking a circular trajectory.

## 🛠️ Step-by-Step Configuration Guide

To run the simulation, open `multiagents_cond.m`. At the very top of the script, you will find the **Main Control Panel** (`0. PANNELLO DI CONTROLLO PRINCIPALE`). 
Change the variables in this section to create your custom scenario:

### Step 1: Set the Network Topology (`STRUTTURA_RETE`)
How should the robots communicate?
* **Set to `1`** -> **Chain Topology:** R1 → R2 → R3 (R3 only receives data from R2).
* **Set to `2`** -> **Bidirectional Topology:** R1 → R2 ↔ R3 (R2 and R3 share data in both directions).

### Step 2: Set the Controller (`STRATEGIA_CONTROLLO`)
Which algorithm should drive the robots?
* **Set to `1`** -> **PD (Loop-Shaping):** A dynamic compensator. Fast, but fails to reject constant wind.
* **Set to `2`** -> **PID (Loop-Shaping):** The most robust option! Includes integral action on the leader to perfectly reject constant environmental disturbances.
* **Set to `3`** -> **State-Feedback (LQR):** Optimal Riccati control with a Luenberger observer. Works flawlessly, but only in ideal laboratory conditions.

### Step 3: Set the Environmental Disturbances (`SCENARIO_SIMULAZIONE`)
The leader always tracks a predefined circular trajectory, but you can inject external forces into the environment to test the controllers' robustness.
* **Set to `1`** -> **No Disturbances:** Ideal vacuum conditions.
* **Set to `2`** -> **Constant Disturbance:** Injects a persistent force (e.g., steady wind or a sloped terrain).
* **Set to `3`** -> **Sinusoidal Disturbance:** Injects oscillating external forces.

### Step 4: Toggle Hardware Realism
Turn these physical constraints ON (`true`) or OFF (`false`):
* **`ACTIVATE_BOUNDS`** -> Set to `true` to simulate physical actuator saturation (motors have a maximum force limit).
* **`USE_NOISE`** -> Set to `true` to inject high-frequency stochastic noise into the position sensors.

---

## 🎮 Ready-to-Run Recipes

If you want to quickly test the most interesting behaviors of the system, copy these exact configurations into your Control Panel:

### 🌟 Recipe A: The "Perfect World" (Ideal Baseline)
Watch the State-Feedback controller perform perfectly without any external interference.
```matlab
STRUTTURA_RETE = 1;
ACTIVATE_BOUNDS = false;
STRATEGIA_CONTROLLO = 3; 
SCENARIO_SIMULAZIONE = 1; % Clean environment
USE_NOISE = false;
```

### 🌪️ Recipe B: The "Static Failure"
Keep the State-Feedback controller but introduce a constant wind. You will see the formation fail to track the target.
```matlab
STRUTTURA_RETE = 1;
ACTIVATE_BOUNDS = true;
STRATEGIA_CONTROLLO = 3; 
SCENARIO_SIMULAZIONE = 2; % Constant wind added!
USE_NOISE = true;
```

### 🛡️ Recipe C: The "Robust Final Boss"
Activate the PID Loop-Shaping controller in the exact same windy conditions as Recipe B. Watch the integral action completely reject the wind and force the robots back onto the perfect track.
```matlab
STRUTTURA_RETE = 1;
ACTIVATE_BOUNDS = true;
STRATEGIA_CONTROLLO = 2;  % PID active!
SCENARIO_SIMULAZIONE = 2; % Constant wind rejected
USE_NOISE = true;
```

---

## 📈 What to expect after running

Once you press **Run**, MATLAB will calculate the physics and automatically open:
1. **XY Trajectory Plot:** A map showing the global movement of the fleet maintaining the triangular formation (ell = 2m).
2. **Error Graphs:** Real-time plots of the tracking error and formation errors converging over time.
3. **Control Efforts:** Graphs of the raw forces (Newtons) requested by the controllers.
4. **Cinematic Animation:** A real-time, dark-themed 2D animation showing the robots physically converging, tracking the target, and leaving historical trails. *(Tip: You can change the playback speed by modifying the `speed_factor` variable at the bottom of the script).*

---

*Developed by: Daniele Buson, Calogero Falco Abramo, Tommaso Franzoni, Alex Martinelli.*  
*Final Project for the **Modeling and Control of Cyber-Physical Systems (CPSs)** course at **Politecnico di Torino**.*
