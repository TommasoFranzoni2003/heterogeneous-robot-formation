# 🤖 Distributed Formation Control: Simulation Tutorial

This repository contains the MATLAB simulation for a distributed control architecture, designed to make a fleet of heterogeneous mobile robots move in a rigid triangular formation while tracking a circular trajectory.

## 🛠️ Step-by-Step Configuration Guide

To run the simulation, open `multiagents_cond.m`. At the very top of the script, you will find the **Main Control Panel** (`0. PANNELLO DI CONTROLLO PRINCIPALE`). Change the variables in this section to create your custom scenario:

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
Watch how the basic PD controller fails to maintain the formation when hit by a constant environmental disturbance (like wind).
```matlab
STRUTTURA_RETE = 1;
ACTIVATE_BOUNDS = true;
STRATEGIA_CONTROLLO = 1; % PD Controller
SCENARIO_SIMULAZIONE = 2; % Constant wind
USE_NOISE = false;
```

---

## 🚀 Execution & Results

Running the simulation is a two-step process. 

### 1. Run the Main Simulation
First, run the `multiagents_cond.m` script. When the computation is complete, the system will **automatically** do two things:

1. **Command Window Output:** The numerical values of the final errors will be printed in the MATLAB console.
2. **Static Plots:** Three figures will open to summarize the results. **Figure 1** shows the trajectories and spatial paths of the robots. **Figure 2** displays the tracking errors over time. **Figure 3** shows the physical control efforts $u = [u_x, u_y]$.

### 2. Run the Animation
The visual animation of the robots moving in formation **does NOT start automatically**. To view the actual dynamic behavior:

1. Ensure the main simulation script has finished and the static figures are open.
2. Open and run `animation.m`.
3. A new figure will pop up showing the real-time movement of the robot fleet.

*Developed by: Daniele Buson, Calogero Falco Abramo, Tommaso Franzoni, Alex Martinelli.*  
*Final Project for the **Modeling and Control of Cyber-Physical Systems (CPSs)** course at **Politecnico di Torino**.*
