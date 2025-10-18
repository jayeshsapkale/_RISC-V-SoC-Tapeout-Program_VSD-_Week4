# Week 4 Task – CMOS Circuit Design (sky130-style)

## 1. Introduction / Background

This experiment explores transistor-level behavior in CMOS circuits using the SKY130 PDK.
It bridges the gap between **device physics** and **Static Timing Analysis (STA)** by simulating
MOSFET characteristics, CMOS inverter behavior, delay, and variation effects.

The experiments include:
1. MOSFET Id–Vds characteristics  
2. Threshold voltage extraction (Id–Vgs)  
3. CMOS inverter VTC (Voltage Transfer Characteristic)  
4. Transient analysis – rise/fall delays  
5. Noise margin extraction  
6. Variation study – VDD and transistor sizing effects  

---

## 2. Experiment 1: MOSFET Behavior (Id vs. Vds)

### Objective
To observe the **linear and saturation regions** of an NMOS transistor by sweeping drain voltage (Vds)
for multiple gate voltages (Vgs).

### SPICE Netlist
```spice
vim day 2_nfet_idvds_LO15_WO39.spice

ngspice day 2_nfet_idvds_L015_W039.spice
plot -vdd#branch
```
<img width="940" height="588" alt="image" src="https://github.com/user-attachments/assets/82de718c-4ab8-4578-b938-1fe76fa3e725" />

<img width="940" height="590" alt="image" src="https://github.com/user-attachments/assets/94b575c9-d496-46ef-8e5b-9626395d801f" />

## Observations

Linear region occurs when Vds < (Vgs - Vt)
Saturation begins when Vds ≥ (Vgs - Vt)
As Vgs increases, Id increases exponentially before saturation

## 3. Experiment 2: Threshold Voltage Extraction (Id vs. Vgs)

### Objective

To determine threshold voltage (Vt) using linear extrapolation from the Id–Vgs curve.

### SPICE Netlist
```
vim day 2_nfet_idvgs_LO15_WO39.spice

ngspice day 2_nfet_idvgs_L015_W039.spice
plot -vdd#branch
```
<img width="940" height="594" alt="image" src="https://github.com/user-attachments/assets/86ad882a-f78d-4794-a2e0-e5c9bcd1483d" />
<img width="940" height="591" alt="image" src="https://github.com/user-attachments/assets/8624ef67-f332-4b44-b49f-e97aa95948e4" />

### Observations
Velocity saturation noticeable for short-channel devices

## 4. Experiment 3: CMOS Inverter VTC

### Objective
To study inverter Voltage Transfer Characteristic (VTC) and identify switching threshold (Vm).

### spicenetlist
```
vim day 3_inv_vtc_Wp084_Wn036.spice

ngspice day 3_inv_vtc_Wp084_Wn036.spice
plot out vs in
```
<img width="940" height="592" alt="image" src="https://github.com/user-attachments/assets/26f92899-0fe4-4f1b-b677-b0396be5aa43" />
<img width="940" height="588" alt="image" src="https://github.com/user-attachments/assets/8fcc51f3-8235-48f9-a5da-3a35adf152d3" />

### Observations

The inverter switches near VDD/2.
The PMOS sizing (2× NMOS width) ensures symmetrical switching.

## 5. Experiment 4: Transient Analysis (Rise/Fall Delays)

### Objective
To find propagation delays using a pulse input.

### SPICE Netlist
```
vim day3_inv_tran_Wp084_Wn036.spice

ngspice day3_inv_tran_Wp084_Wn036.spice
plot out vs in
```
<img width="940" height="594" alt="image" src="https://github.com/user-attachments/assets/93aae014-6ad9-422e-96f8-c60a09576b98" />
<img width="940" height="596" alt="image" src="https://github.com/user-attachments/assets/1ad2e79b-03cc-4814-9121-9ebaa602c6f3" />

### Observations
The rise delay (PLH) > fall delay (PHL) due to PMOS having lower mobility.

## 6. Experiment 5: Noise Margin Analysis

### Objective
To calculate noise margins from the VTC curve.

### spice netlist
```
vim day4_inv_noisemargin_Wp1_wn036.spice

ngspice day4_inv_noisemargin_Wp1_wn036.spice
plot out vs in
```
<img width="940" height="593" alt="image" src="https://github.com/user-attachments/assets/6a87865d-727c-4de6-b453-7f0299fd1998" />
<img width="940" height="588" alt="image" src="https://github.com/user-attachments/assets/3768d38f-2ec2-4e7f-a4b1-fcc17cab210b" />

## 7. Experiment 6: Power Supply & Device Variation

### Objective
To study the effect of VDD and transistor sizing on switching threshold and noise margin.

### SPICE Netlist (VDD Sweep)
<img width="940" height="577" alt="image" src="https://github.com/user-attachments/assets/a6106858-3eee-4aed-904f-d2abc7c89eac" />
<img width="940" height="594" alt="image" src="https://github.com/user-attachments/assets/374ab76b-3dd6-4a2c-978d-bc4267cbb47b" />
<img width="940" height="591" alt="image" src="https://github.com/user-attachments/assets/2b7ef0e5-7aff-43b9-bd4d-63cf258d3d62" />
<img width="940" height="595" alt="image" src="https://github.com/user-attachments/assets/34155bd8-46a9-4429-bc7e-e243b5e56934" />

### Observation
Decreasing VDD shifts switching threshold lower and increases delay.
Increasing PMOS width improves logic high strength but increases capacitance.

### Analysis & Discussion
Device Physics: Increasing Vgs increases inversion channel charge, boosting Id until velocity saturation limits current.
STA Link: The transistor-level propagation delays correspond to the “cell delay” STA models.
Variation Impact: Changes in VDD or device W/L directly affect delay and slack — illustrating STA’s need for margining.
Symmetry: Proper PMOS sizing ensures balanced rise/fall times and symmetrical VTC.

### Conclusion
This CMOS circuit design study using the SKY130 PDK demonstrates:
How transistor parameters affect circuit behavior
How VTC and delay depend on device sizing and VDD
How noise margins ensure logic reliability
The connection between physical transistor limits and STA timing constraints
