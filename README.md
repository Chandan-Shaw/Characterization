# IC-Characterization
- This GitHub repository is created to provide hands-on experience in Analog IC Characterization using open-source tools such as **NGSpice**, **Xschem**, and the **SKY130 PDK**.
--------------------------------------------------------------------------------------------------------
## What Is Analog Characterization ?
Analog IC (Integrated Circuit) Characterization is the process of evaluating and measuring the electrical performance of an analog circuit under various conditions. This involves testing key parameters such as gain, offset, bandwidth, noise, power consumption, input/output impedance, temperature stability, and process variation sensitivity.

Characterization is carried out using tools like Ngspice, Eldo (Simulation) and Xschem (Schematic) providing a complete framework for accurate simulation and analysis of analog circuits.

Characterization is usually performed post-design (pre- and post-fabrication) to ensure the circuit behaves as intended across different corners:

- Process Corners: Variations in manufacturing (e.g., TT, FF, SS).

- Voltage Corners: Operating at min/max supply voltages.

- Temperature Corners: Measuring across temperature range (e.g., -40°C to 125°C).

--------------------------------------------------------------------------------------------------------
# Content
## 1. Tools And PDK SetUp
 
 ## 1.1 Tools Required
 For the simulation of circuits we will need the following tools.
 - Spice netlist simulation - [[Ngspice](https://ngspice.sourceforge.io/)]
 - Schematic Editor - [[Xschem](https://xschem.sourceforge.io/stefan/index.html)]

### Ngspice
![Diagram](https://github.com/Chandan-Shaw/Characterization/blob/main/Ngspice.png)

[Ngspice](http://ngspice.sourceforge.net/devel.html) is the open source spice simulator for electric and electronic circuits. Ngspice is an open project, there is no closed group of developers.

[Ngspice Reference Manual](https://ngspice.sourceforge.io/docs/ngspice-html-manual/manual.xhtml): Complete reference manual in HTML format.

### Xschem
![Diagram](https://github.com/Chandan-Shaw/Characterization/blob/main/Xschem.png)

[Xschem](https://xschem.sourceforge.io/stefan/) is an open-source schematic capture tool for VLSI and electronics. It is designed to be lightweight, fast, and capable of handling large hierarchical circuits while remaining user-friendly.

[Xschem Reference Manual](https://xschem.sourceforge.io/stefan/xschem_man/xschem_man.html): Complete reference manual in HTML format.

## 1.2 PDK Required

A process design kit (PDK) is a set of files used within the semiconductor industry to model a fabrication process for the design tools used to design an integrated circuit. The PDK is created by the foundry defining a certain technology variation for their processes. It is then passed to their customers to use in the design process.

The PDK we are going to use is [Google Skywater 130nm PDK](https://skywater-pdk.readthedocs.io/en/main/).

![Diagram](https://github.com/Chandan-Shaw/Characterization/blob/main/SKY130_PDK.png)

Device Details: [docs](https://skywater-pdk.readthedocs.io/en/main/rules/device-details.html)

## 1.3 Install and Setup EDA Tools
## Windows Subsystem for Linux (WSL) for Open Source EDA tools

Windows Subsystem for Linux (WSL) is a feature of Windows that allows you to run a Linux environment on your Windows machine, without the need for a separate virtual machine or dual booting. With native X11 (graphics) support on WSL2, the latest WSL, in **Winodws 10 version 2004+ (Build 19041+)** or **Windows 11**, you can now run GUI apps including all the open-source EDA tools.

Now we will share instructions for installing WSL2 on Winodws 10/11 and install the EDA tools on a **Ubuntu 24.04** distribution.

## Install WSL
- **Prerequisites**: Winodws 10 version 2004+ (Build 19041+) or Windows 11
- If you have a previous WSL installed without your knowledge or, you've installed in from the _Microsoft Store_, it's best you **uninstall** it using the Windows "Add/remove Programs" app and/or `wsl --uninstall`
- Open PowerShell or Windows Command Prompt in **ADMINISTRATOR** mode by right-clicking and selecting "Run as administrator"
- In the PowerShell type `wsl --list --online`
  - This will list all the available distributions online. 
- To install a particular distribution (Distro) say `Ubuntu-24.04` (The name has to be exactly as printed in the above command):
  - `wsl --install -d Ubuntu-24.04`
- It's important to update the WSL now by typing the following in the Powershell:
  - `wsl --update`
- And shut it down: `wsl --shutdown`. Note: it will automatically start when the WSL distro selected from the Windows menu.

## Launching Ubuntu 24.04. 

- `Press Windows Key → select Ubuntu 24.04.`
- Update the system
```
sudo apt update && sudo apt upgrade -y
```
- Clone the Github Repository
```
cd ~
git clone https://github.com/silicon-vlsi/SI-2025-AnalogIC.git

```
- Copy & make install scripts executable
```
cp ~/SI-2025-AnalogIC/install*.sh .
chmod +x install*.sh
```
- Install dependencies & EDA tools
```
./install-libs.sh
./install-eda.sh
```
- Add EDA environment variables
```
cat ~/SI-2025-AnalogIC/bashrc-eda >> ~/.bashrc
source ~/.bashrc
```
- Setup Xschem initialization
```
cp ~/share/xschemrc ~/.xschem/xschemrc
```
- Then type this command: 
``` 
tree -L 2
```
**Directory Structure** after installation should look like this:
```bash
share
└── pdk
cad
├── eda-magic
├── eda-netgen
├── eda-ngspice
└── eda-xschem
work
└── xschem
.xschem/
└── simulations
```

## 2. Basic Concept

### 2.1 Charge
 The most basic quantity in an electric circuit is the electric charge.
  
 Charge is an **Electric Property** of an atomic particle of which matter consists, measured in **Coulombs** (C).
  
 The Coulomb is a large unit of charge. ``1C = 1 / 1.602 * 10^-19 =  6.24 * 10^-18``  

### 1.2 Current
 The electric current due to the flow of electronic charge in a conductor.

 Electric Current is the time rate of change of Charge,measured in **Ampere** (A).

 Mathematically - `` i = dq/dt ``
 
 ( 1 Ampere = 1 Coulomb/1Second )

### 2.3 Voltage
Voltage is the energy required to move a unit charge from a reference point(-) to another point (+), measured in
**Volts (V).**

Voltage between two pont a & b is as:  `` Vab = dw/dq ``  ( 1 Volt = 1 Joule/Coulomb )

**Vab = -Vba** (Voltage drops from a to b is equivalent to voltage rise from b to a )

### 2.4 Power
 Power is the time rate of expanding or absorbing energy, measured is **Watts (W).**

 Mathematically , `` p = dw/dt `` (1 Watt = 1 Joule/1Second)

 where p is **power**, w is **energy** in Joules and t is the **time** in seconds.

 `` p = vi `` , is a time varing quantity and is called **Instantaneous Power.**

 The power absorbed or supplied by an element is the product of the voltage across the element and the current          through it.

### 2.5 Energy
 Energy is the capacity to do work, measured in **Joules (J).**

 Energy is an electric circuit is defined as the total power consumption in a period (From time t0 to t)

 Mathematically ,  w = $\int_{t_0}^{t} p \ dt$ = $\int_{t_0}^{t} vi \ dt$  

 The electric power utility companies measures energy in Watt-hour (Wh) , Where `` 1 Wh = 3600 J `` 
 
## 3. Linear Element
  
### 3.1 Resistor
 A resistor is an passive electrical component that opposes the flow of electric current.

 The resistance value is measured in **Ohms (Ω)**
 
 Given by Ohm’s Law:   `` V=I×R `` 
 
 **Important Point** :- By the resistance we can control the current.
  
 **Used of Resistor**
 - Its main use is to limit or control the flow of electric current in a circuit.
 - To set biasing in amplifiers
 
### Types of Resistors available :
- ``sky130_fd_pr__res_high_po.model`` has base models with *0.35u, 0.69u, 1.41u, 2.85u, 5.73u* as **bin width** (fixed) with changable lengths. 
- ``sky130_fd_pr__res_xhigh_po.model`` also has base models with *0.35u, 0.69u, 1.41u, 2.85u, 5.73u* as **bin width** (fixed) with changable lengths.
- ``sky130_fd_pr__res_generic_nd.model`` is a Generic N-diff type resister.
- ``sky130_fd_pr__res_generic_pd.model`` is a Generic P-diff type resister.

![Diagram](https://github.com/Chandan-Shaw/Characterization/blob/main/Resistor.jpg)

#### Netlist Code Of Resistor Simulation
```
************************ Resistor Simulation *********************
***** Date: 01/01/2026, Designer: Chandan Shaw , Silicon University, Bhubaneshwar ******

.title Resistor Simulation
.lib /home/chandanvlsi/share/pdk/sky130A/libs.tech/ngspice/sky130.lib.spice tt
.temp 25

Vin     in      0       DC 1.8
Vcm      in      1       0
X1      1       0       vdd   sky130_fd_pr__res_high_po_0p35 L=3.5
vsup    vdd     gnd     DC 1.8
.dc Vin 0 1.8 0.01

.op
.control
run
print v(in)
print abs(i(Vcm))
let RES = v(in)/abs(i(Vcm))
print RES
.endc
.end
```

![Diagram](https://github.com/Chandan-Shaw/Characterization/blob/main/Resistor%20Value.png)

```
*********************** Characterization Of Resistance *********************
********* Designer: Chandan Shaw, Silicon University Bhubaneshwar **********

.global vdd gnd
.temp 27

R1 1 gnd 1k
Vin in gnd dc 1
Vcm in 1 dc 0
.dc vin 0 1 0.01
.control
run
plot i(Vcm) xlabel 'Temperature' ylabel 'Current (mA)'
.endc
.end
```

![Diagram](https://github.com/Chandan-Shaw/Characterization/blob/main/Resistor%20Simulation.png)

### 3.2 Capacitors
 A capacitor is a passive electrical component that stores energy in the form of an electric field, defined by the relation: `` Q = C * V ``, where C is the capacitance in **Farads**.

 The capacitance C of a parallel-plate capacitor depends on its physical structure and the material between the plates, given by the formula: `` C = εA / d ``.

 In the **Skywater SKY130 PDK**, various capacitor types are available for use in analog, RF, and digital designs, each offering trade-offs in capacitance density, linearity, voltage rating, and            temperature stability.

### Types of Capacitors available:
 - ``sky130_fd_pr__cap_mim_m3_1.model`` is a **Metal-Insulator-Metal (MIM)** capacitor between **Metal3 and                  Metal2**, suitable for analog precision applications.
 - ``sky130_fd_pr__cap_mim_m3_2.model`` is another **MIM** capacitor variant with different area usage and                   parasitic trade-offs.
 - ``sky130_fd_pr__cap_mim_m2_1.model`` defines a MIM capacitor between **Metal2 and Metal1** layers.
 - ``sky130_fd_pr__cap_var_lvt.model`` is a **MOS varactor** (voltage-dependent capacitor) built using LVT NMOS              structure, useful for RF tuning.
 - ``sky130_fd_pr__cap_var_hvt.model`` is a similar **varactor** using HVT device for different threshold and                leakage behavior.

#### Circuit Diagram Of RC Charging Circuit With Pulse Input
 ![Diagram](https://github.com/Chandan-Shaw/Characterization/blob/main/Capacitor.JPG)

#### Netlist Code Of Circuit Diagram
 ```
 ************************ RC Charging Circuit With Pulse Input **********************
 ********* Date: 01/01/2026 , Designer: Chandan Shaw , Silicon University  **********

.title RC Charging Circuit With Pulse Input
.lib "/home/chandanvlsi/share/pdk/sky130A/libs.tech/ngspice/sky130.lib.spice tt"
.temp 25

V1      in      0       pulse(0 1.8 0 1p 1p 100p 200p)
R1      in      out     4.45k
XC1     out     0       sky130_fd_pr__cap_mim_m3_1 w=1 l=1

.tran 0.1n 1n

.control
run
plot v(in) v(out)
.endc

.end
```

#### Plot Of Vin And Vout
  
![Diagram](https://github.com/Chandan-Shaw/Characterization/blob/main/RC_Simulation.png)

### 3.3 RC Circuit
  - An RC circuit is an electrical circuit made of **R — Resistor** & **C — Capacitor** connected either in series or parallel, which exhibit a time-dependent response to voltage or current changes. The     fundamental time constant is defined as:
     `τ = R * C`, where `τ` (tau) represents the **time constant** in seconds, indicating how quickly the circuit charges or discharges.
- In the **Skywater SKY130 PDK**, **RC circuits** are implemented using integrated resistors (e.g.,`sky130_fd_pr__res_high_po`) and capacitors (e.g.,`sky130_fd_pr__cap_mim_m3_1`). These are critical in    analog and mixed-signal design applications such as filters, timing circuits, and analog front ends.

 ![Diagram](https://github.com/Chandan-Shaw/Characterization/blob/main/RC%20Circuit.JPG)
 
  #### 3.3.1 Transient Analysis
  ```
  ********************** RC Charging Circuit In Transient Analysis *******************
  ********* Date: 02/01/2026 , Designer: Chandan Shaw , Silicon University  **********

  .title RC Circuit
  .lib /home/chandanvlsi/share/pdk/sky130A/libs.tech/ngspice/sky130.lib.spice tt
  .temp 25

  Vin     in      0       PULSE(0 1.8 0 0 0 100p 200p)
  XR1     in      out     0       sky130_fd_pr__res_high_po_0p35 l =3.5
  XC1     out     0       sky130_fd_pr__cap_mim_m3_1 w=1 l=1

  .tran 1p 300p

  .control
  run
  plot v(in) v(out)
  .endc

  *Measure Time delays
  .meas tran rise        TRIG V(out) VAL=0.18 RISE=1 TARG V(out) VAL=1.62 RISE=1 ; rise‑time 10 % → 90 % at V(out)
  .meas tran fall        TRIG V(out) VAL=1.62 FALL=1 TARG V(out) VAL=0.18 FALL=1 ; fall‑time 90 % → 10 % at V(out)
  .meas tran rise_delay  TRIG V(in)  VAL=0.9  RISE=1 TARG V(out) VAL=0.9  RISE=1 ; tpd (low→high) 50 % V(in) → 50 % V(out)
  .meas tran fall_delay  TRIG V(in)  VAL=0.9  FALL=1 TARG V(out) VAL=0.9  FALL=1 ; tpd (high→low) 50 % V(in) → 50 % V(out)

  *Measure Max Voltage
  .meas tran VMAX MAX V(out) ; peak V(out) during transient

  .end
  ```
 
  #### Simulation Of Transient Analysis
  ![Diagram](https://github.com/Chandan-Shaw/Characterization/blob/main/RC_Transient_Simulation.png)
    
  #### 3.3.2 AC Analysis
  ```
  ***************************** RC Circuit In AC Analysis ****************************
  ********* Date: 02/01/2026 , Designer: Chandan Shaw , Silicon University  **********

  .title RC Circuit
  .lib "/home/chandanvlsi/share/pdk/sky130A/libs.tech/ngspice/sky130.lib.spice" tt
  .temp 25

  V1      in      0       AC 1
  XR1     in      out     0       sky130_fd_pr__res_high_po_0p35  l=3.5
  XC1     out     0       sky130_fd_pr__cap_mim_m3_1 w=1 l=1

  * AC Simulation
  .ac dec 10 1 15g

  * Output commands
  .control
  run
  .meas ac f3db WHEN VDB(out) = -3 ; –3 dB cutoff frequency
  plot vdb(out)
  .endc

  .end
  ```
  #### Simulation Of AC Analysis
  ![Diagram]( https://github.com/Chandan-Shaw/Characterization/blob/main/RC_AC_Simulation.png)

  ### 3.4 CR Circuit
  - A **Cr circuit** is essentially the same as an RC circuit, but with the capacitor (C) placed before the resistor (R) in the signal path. While electrically the time constant remains the same, the        circuit response differs, especially in transient analysis. The fundamental time constant is defined as: `τ = R * C`,
  where `τ` (tau) represents the **time constant** in seconds, indicating how quickly the circuit charges or discharges.

  - In the **Skywater SKY130 PDK**, **CR circuits** are implemented using integrated capacitors (e.g., `sky130_fd_pr__cap_mim_m3_1`) and resistors (e.g., `sky130_fd_pr__res_high_po`). These configurations   are often used in differentiator circuits, pulse shaping, and AC coupling applications in analog and RF systems.

  ![Diagram](https://github.com/Chandan-Shaw/Characterization/blob/main/CR%20Circuit.JPG)

  #### 3.4.1 Transient Analysis
  ```
  ************************** CR Circuit In Transient Analysis *************************
  ********* Date: 03/01/2026 , Designer: Chandan Shaw , Silicon University  ***********

  title CR Circuit
  .lib /home/chandanvlsi/share/pdk/sky130A/libs.tech/ngspice/sky130.lib.spice tt
  .temp 25

  Vin     in      0       PULSE(0 1.8 0 0 0 100p 200p)
  XC1     in      out     sky130_fd_pr__cap_mim_m3_1 w=1 l=1
  XR1     out     0       0       sky130_fd_pr__res_high_po_0p35 l =3.5

  .tran 1p 300p

  .control
  run
  plot v(in) v(out)
  .endc

  *Measure Time delays
  .meas tran rise TRIG V(out) VAL=0.14 RISE=1 TARG V(out) VAL=1.29 RISE=1
  .meas tran fall TRIG V(out) VAL=1.29 FALL=1 TARG V(out) VAL=0.14 FALL=1
  .meas tran rise_delay TRIG V(in) VAL=0.7 RISE=1 TARG V(out) VAL=0.7 RISE=1
  .meas tran fall_delay TRIG V(in) VAL=0.7 FALL=1 TARG V(out) VAL=0.7 FALL=1

  *Measure Max Voltage
  .meas tran VMAX MAX V(out)

  .end
  ```
  #### Simulation Of Transient Analysis
  ![Diagram](https://github.com/Chandan-Shaw/Characterization/blob/main/CR_Transient_Simulation.png)

  #### 3.4.2 AC Analysis

  ```
  ***************************** CR Circuit In AC Analysis *****************************
  ********* Date: 03/01/2026 , Designer: Chandan Shaw , Silicon University  ***********

  title CR Circuit
  .lib "/home/chandanvlsi/share/pdk/sky130A/libs.tech/ngspice/sky130.lib.spice tt"
  .temp 25
  
  V1       in     0           ac  1
  XC1     in     out          sky130_fd_pr__cap_mim_m3_1  w=1 l=1
  XR1     out     0       0   sky130_fd_pr__res_high_po_0p35  l=3.5
  * Simulation Command  
  .ac dec 10 1meg 10e13
  
  .control
  run
  plot vdb(out)
  .endc
  .end
  ```

  #### Simulation Of AC Analysis
  ![Diagram](https://github.com/Chandan-Shaw/Characterization/blob/main/CR_AC_Simulation.png)

 ## 4. MOSFET Circuits
 - A MOSFET(Metal-Oxide-Semiconductor Field-Effect Transistor) has four terminal device:- Gate, Drain, Source, Body.
 - MOSFET is device used for switching and amplification, Its current is controlled by the voltage applied to the gate terminal.
 - The MOSFET operates in three regions: **cutoff, linear, and saturation**, depending on gate-source (VGS) and drain-source (VDS) voltages.
 - In the Skywater SKY130 PDK, MOSFETs like ``sky130_fd_pr__nfet_01v8`` (NMOS) and ``sky130_fd_pr__pfet_01v8`` (PMOS) are commonly used. These are essential in digital logic, analog amplifiers, and switching applications.

**Important Point** :- MOSFET does not behave like a Inductor, But we can design a inductor by the help of MOSFET Circuit

### 4.1 NMOS Analysis

- A **NMOS** (N-type MOSFET) is a majority-carrier device where current flows between the drain and source when a positive voltage is applied to the gate. It acts as a voltage-controlled current source.
- The drain current (I<sub>D</sub>) depends on the gate-to-source voltage (V<sub>GS</sub>), and its behavior changes across three regions:
- Cutoff: V<sub>GS</sub> < V<sub>th</sub>, I<sub>D</sub> ≈ 0
- Linear: V<sub>GS</sub> > V<sub>th</sub> and V<sub>DS</sub> < V<sub>GS</sub> − V<sub>th</sub>
- Saturation: V<sub>DS</sub> ≥ V<sub>GS</sub> − V<sub>th</sub>
- The I<sub>D</sub>-V<sub>GS</sub> curve shows how the drain current increases with gate voltage (at constant V<sub>DS</sub>), helping identify the threshold voltage (V<sub>th</sub>), where the transistor starts conducting. This     curve is essential for characterizing the device and is often used in DC sweep simulations.
- In the Skywater SKY130 PDK, NMOS devices like `sky130_fd_pr__nfet_01v8` are used in logic gates, analog blocks, and current sources.

#### Netlist Code Of NMOS Analysis for ID vs VGS Curve

```
****************************** NMOS Analysis Of MOSFET **************************************
******** Date: 20/12/2025 , Designer: Chandan Shaw , Silicon University, Bhubaneswar ********

.lib /home/chandanvlsi/share/pdk/sky130A/libs.tech/ngspice/sky130.lib.spice tt
.temp 27

Vd      1       0       DC 1.8
Vid     1       d       DC 0
Vg      g       0       DC 0

* NMOS: D G S B
X1      d       g       0       0       sky130_fd_pr__nfet_01v8 w=0.42 l=1

.control
run
save all

dc vg 0 1.8 0.001 vd 0 1.8 0.1
plot  I(vid) xlabel "VGS (V)"  ylabel "ID (A)" title "ID vs VGS"

.endc
.end
```
![Diagram](https://github.com/Chandan-Shaw/Characterization/blob/main/NMOS_Id_Vgs%20Curve.png)

#### Netlist Code Of NMOS Analysis for ID vs VDS Curve

```
****************************** NMOS Analysis Of MOSFET **************************************
******** Date: 20/12/2025 , Designer: Chandan Shaw , Silicon University, Bhubaneswar ********

.lib /home/chandanvlsi/share/pdk/sky130A/libs.tech/ngspice/sky130.lib.spice tt
.temp 27

Vd      1       0       DC 1.8
Vid     1       d       DC 0
Vg      g       0       DC 0

* NMOS: D G S B
X1      d       g       0       0       sky130_fd_pr__nfet_01v8 w=0.42 l=1

.control
run
save all

dc vd 0 1.8 0.001 vg 0 1.8 0.1
plot  I(vid) xlabel "VDS (V)"  ylabel "ID (A)" title "ID vs VDS"

.endc
.end
```
![Diagram](https://github.com/Chandan-Shaw/Characterization/blob/main/NMOS_Id_Vds%20Curve.png)

#### Netlist Code Of PMOS Analysis for ID vs VGS 

```
****************************** PMOS Analysis Of MOSFET **************************************
******** Date: 20/12/2025 , Designer: Chandan Shaw , Silicon University, Bhubaneswar ********

.lib /home/chandanvlsi/share/pdk/sky130A/libs.tech/ngspice/sky130.lib.spice tt
.temp 27

* Bias sources
VS      s       0       1.8 ; Source at 1.8 V
VG      g       0       0   ; Gate
VD      d       0       0   ; Drain at 0 V

* PMOS: D  G  S  B
X1 d g s s sky130_fd_pr__pfet_01v8 w=1 l=0.15

.control
run
save all

*ID vs VGS Curve
dc VG 0 1.8 0.01 VD 0 1.8 0.1
plot I(VD) xlabel "VGS (V)" ylabel "ID (A)" title "PMOS ID vs VGS"

.endc
.end
```
![Diagram](https://github.com/Chandan-Shaw/Characterization/blob/main/PMOS_Id_Vgs%20Curve.png)

#### Netlist Code Of PMOS Analysis for ID vs VDS Curve

```
****************************** PMOS Analysis Of MOSFET **************************************
******** Date: 20/12/2025 , Designer: Chandan Shaw , Silicon University, Bhubaneswar ********

.lib /home/chandanvlsi/share/pdk/sky130A/libs.tech/ngspice/sky130.lib.spice tt
.temp 27

* Bias sources
VS      s       0       1.8 ; Source at 1.8 V
VG      g       0       0   ; Gate
VD      d       0       0   ; Drain at 0 V

* PMOS: D  G  S  B
X1 d g s s sky130_fd_pr__pfet_01v8 w=1 l=0.15

.control

run
save all

*ID vs VDS Curve
dc VD 0 1.8 0.01 VG 0 1.8 0.1
plot I(VD) xlabel "VDS (V)" ylabel "ID (A)" title "PMOS ID vs VDS"

.endc
.end
```
![Diagram](https://github.com/Chandan-Shaw/Characterization/blob/main/PMOS_Id_Vds%20Curve.png)

## 5. INVERTER

### 5.1 CMOS Inverter

A **CMOS inverter** using the open-source **SkyWater SKY130 PDK**, with `sky130_fd_pr__pfet_01v8_lvt` (pMOS) and `sky130_fd_pr__nfet_01v8_lvt` (nMOS) transistors. The inverter is designed and simulated in **Xschem + Ngspice**, showing correct switching behavior from 0 V to 1.8 V. Key performance aspects such as transfer characteristics, switching threshold, and rise/fall times are verified.

### Key Points  
- **Technology:** SKY130 (1.8 V low-Vt devices)  
- **Transistors Used:** `pfet_01v8_lvt` (PMOS), `nfet_01v8_lvt` (NMOS)  
- **Simulation Tools:** Xschem (schematic) + Ngspice (simulation)  
- **Input:** Pulse source (0 → 1.8 V)  
- **Output:** Clean digital inversion with sharp transition near VDD/2
  
### Why CMOS is important ?
- Very Low Power Consumption
- High Speed
- High Packing Density ( Many Circuits on a small Chip. )
- Reliability

It is used in like ***Microprocessors***,***Smartphone***,***Digital Logic Circuits***,***Memory Chips.***

### Netlist Code CMOS Inverter In DC Analysis

```
****************************** CMOS Inverter DC Analysis **************************************
******** Date: 20/12/2025 , Designer: Chandan Shaw , Silicon University, Bhubaneswar **********
.lib /home/chandanvlsi/share/pdk/sky130A/libs.tech/ngspice/sky130.lib.spice tt
.temp 25

VDD ps 0 DC 1.8
VIN in 0 DC 0
XM1 out in 0 0 sky130_fd_pr__nfet_01v8 W=0.42 L=0.15
XM2 out in ps ps sky130_fd_pr__pfet_01v8 W=1.26 L=0.15

** Simulation Command
.dc VIN 0 1.8 0.01

.control
run
plot  v(in) v(out)
.endc
.end
```
#### Simulation Of DC Analysis
![Diagram](https://github.com/Chandan-Shaw/Characterization/blob/main/CMOS_Inverter_DC_Simulation.png)

### Netlist Code CMOS Inverter In AC Analysis

```
****************************** CMOS Inverter AC Analysis **************************************
******** Date: 20/12/2025 , Designer: Chandan Shaw , Silicon University, Bhubaneswar **********
.lib /home/chandanvlsi/share/pdk/sky130A/libs.tech/ngspice/sky130.lib.spice tt
.temp 25

Vdd Vdd 0 1.8
Vin In  0 1.8 ac  1
XM1 Out In Vdd Vdd sky130_fd_pr__pfet_01v8_lvt w=7 l=.35
XM2 Out In 0   0   sky130_fd_pr__nfet_01v8_lvt w=7 l=.15
C1 out 0 100f

** Simulation Command Of AC Analysis
.ac dec 10 1meg 10e13

** Control Command
.control
run
plot vdb(out)

.endc
.end
```
#### Simulation Of AC Analysis
![Diagram](https://github.com/Chandan-Shaw/Characterization/blob/main/CMOS_Inverter_AC_Simulation.png)
### Netlist Code CMOS Inverter In Transient Analysis

```
****************************** CMOS Inverter Transient Analysis *************************************
******** Date: 20/12/2025 , Designer: Chandan Shaw , Silicon University, Bhubaneswar ****************
.title CMOS Inverter Transient Analysis
.lib /home/chandanvlsi/share/pdk/sky130A/libs.tech/ngspice/sky130.lib.spice tt
.temp 25

* Supply voltage
VDD vdd 0 DC 1.2
VIN in 0 PULSE(0 1.2 2n 0 0 100n 200n)
XM1 out in 0 0 sky130_fd_pr__nfet_01v8 W=1.26 L=0.15
XM2 out in vdd vdd sky130_fd_pr__pfet_01v8 W=1.26 L=0.15
Cload out 0 100f

** Simulation Command Of Transient Analysis
.tran 0.1n 500n

** Control Command
.control
run
plot v(in) v(out)

.endc
.end
```
#### Simulation Of Transient Analysis
![Diagram](https://github.com/Chandan-Shaw/Characterization/blob/main/CMOS_Inverter_Transient_Simulation.png)

### 5.2 Static Power
#### Netlist Code Of CMOS Inverter Static Power
```
****************************** CMOS Inverter Static Power *******************************************
******** Date: 20/12/2025 , Designer: Chandan Shaw , Silicon University, Bhubaneswar ****************
.title CMOS Inverter Static Power
.lib /home/chandanvlsi/share/pdk/sky130A/libs.tech/ngspice/sky130.lib.spice tt
.temp 25
.global VDD  GND

**PMOS**
VDD      VDD     GND     DC      1.8
VIN      in      GND     DC      0
XP1      out     in      VDD     VDD    sky130_fd_pr__pfet_01v8_lvt L=0.35  W=7
XM1      out     in      GND     GND    sky130_fd_pr__nfet_01v8_lvt L=0.15  W=7
C1      out     0       1a

*NMOS*
Vns     n1      0       DC      1.8
V1      i1      0       DC      1.8
XP2     o1     i1       n1       n1    sky130_fd_pr__pfet_01v8_lvt L=0.35  W=7
XM2     o1     i1       0        0    sky130_fd_pr__nfet_01v8_lvt L=0.15  W=7
C2      o1      0       1a

.OP
.control
run
print abs(I(VDD))
print abs(I(Vns))
let static_power=((I(VDD)) + (I(Vns)))*1.8
print abs(static_power)
.end
.endc
```
#### Simulation Of CMOS Inverter Static Power
![Diagram](https://github.com/Chandan-Shaw/Characterization/blob/main/CMOS_Static_Power_Value.png)
### 5.2 Dynamic Power
#### Netlist Code Of CMOS Inverter Dynamic Power
```
****************************** CMOS Inverter Dynamic Power *******************************************
******** Date: 20/12/2025 , Designer: Chandan Shaw , Silicon University, Bhubaneswar ****************

.title CMOS Inverter Dynamic Power
.lib /home/chandanvlsi/share/pdk/sky130A/libs.tech/ngspice/sky130.lib.spice ss
.temp 125
Vdd Vdd 0 1.8
Vin In 0 PULSE(0 1.8 0 10n 10n 60n 120n)
XM1 Out In Vdd Vdd sky130_fd_pr__pfet_01v8_lvt w=7 l=.35
XM2 Out In 0   0   sky130_fd_pr__nfet_01v8_lvt w=7 l=.15
C1 out 0 1a
.tran 0.1n 240n
.op
.control
run
plot V(in)  V(Out)
plot abs(i(Vdd))
meas tran i(avg) AVG  i(Vdd)
meas tran rise_time TRIG v(out) VAL=0.18 RISE=1 TARG v(out) VAL=1.62 RISE=1
meas tran fall_time TRIG v(out) VAL=1.62 FALL=1 TARG v(out) VAL=0.18 FALL=1
meas tran delay_time TRIG v(in) VAL=0.9 RISE=1 TARG v(out) VAL=0.9 RISE=1
meas tran vmax MAX v(out)
meas tran vmin MIN v(out)
let  power = i(avg)*1.8
print power
.endc
.end
```
#### Simulation Of CMOS Inverter Dynamic Power
![Diagram](https://github.com/Chandan-Shaw/Characterization/blob/main/CMOS_Inverter_Dynamic_Power_Value.png)
![Diagram](https://github.com/Chandan-Shaw/Characterization/blob/main/CMOS_Inverter_Dynamic_Power_Simulation.png)
![Diagram](https://github.com/Chandan-Shaw/Characterization/blob/main/CMOS_Inverter_Dynamic_Power_Simulations.png)

### 5.3 CMOS Inverter Fanout
#### Netlist Code Of CMOS Inverter Fanout
```
****************************** CMOS Inverter Fanout *******************************************
******** Date: 20/12/2025 , Designer: Chandan Shaw , Silicon University,Bhubaneswar ***********

.title CMOS Inverter Fanout
.lib /home/chandanvlsi/share/pdk/sky130A/libs.tech/ngspice/sky130.lib.spice ss
.temp 125

Vdd Vdd 0 1.8
Vin In 0 PULSE(0 1.8 0 10n 10n 60n 120n)
XM1 Out In Vdd Vdd sky130_fd_pr__pfet_01v8_lvt w=7 l=.35 m=1
XM2 Out In 0   0   sky130_fd_pr__nfet_01v8_lvt w=7 l=.15 m=1

XM11 Out1 Out Vdd Vdd sky130_fd_pr__pfet_01v8_lvt w=7 l=.35 m=1
XM21 Out1 Out 0   0   sky130_fd_pr__nfet_01v8_lvt w=7 l=.15 m=1

XM12 Out2 Out Vdd Vdd sky130_fd_pr__pfet_01v8_lvt w=7 l=.35 m=1
XM22 Out2 Out 0   0   sky130_fd_pr__nfet_01v8_lvt w=7 l=.15 m=1

XM13 Out3 Out Vdd Vdd sky130_fd_pr__pfet_01v8_lvt w=7 l=.35 m=1
XM23 Out3 Out 0   0   sky130_fd_pr__nfet_01v8_lvt w=7 l=.15 m=1

XM14 Out4 Out Vdd Vdd sky130_fd_pr__pfet_01v8_lvt w=7 l=.35 m=1
XM24 Out4 Out 0   0   sky130_fd_pr__nfet_01v8_lvt w=7 l=.15 m=1

.tran 0.1n 240n
.op
.control
run
meas tran rise_time TRIG v(out) VAL=0.18 RISE=1 TARG v(out) VAL=1.62 RISE=1
meas tran fall_time TRIG v(out) VAL=1.62 FALL=1 TARG v(out) VAL=0.18 FALL=1
meas tran delay_time TRIG v(in) VAL=0.9 RISE=1 TARG v(out) VAL=0.9 RISE=1
meas tran vmax MAX v(out)
meas tran vmin MIN v(out)
.endc
.end
```
#### Simulation Of CMOS Inverter Fanout
![Diagram](https://github.com/Chandan-Shaw/Characterization/blob/main/CMOS_Inverter_Fanout_Value.png)

## 6. Current Mirror

- A current mirror is an analog circuit that copies (or "mirrors") a reference current from one branch of a circuit into another branch, maintaining a constant output current regardless of the load resistance (within limits).
- It’s widely used in biasing circuits, active loads, and current-mode logic.
- Implemented using matched transistors (BJTs or MOSFETs).
- Key Idea : If two transistors are perfectly matched and have the same VGS then they will conduct the same drain/collector current.
- 
### Advantages
- Accurate current replication (when devices are matched and well-designed).
- High output impedance - good for biasing and active loads.
- Compact design — no need for large resistors.
- Scalable — easy to generate multiple identical or scaled currents.
- Temperature tracking — matched devices track each other’s thermal changes.
### Disadvantages
- Device mismatch causes current errors.
- Finite output resistance leads to current variation.
- Voltage headroom requirement (especially in cascode types).
-  Temperature dependence — though better than resistors, still affected.
- Limited accuracy at very low currents due to leakage and mismatch.

### feature of different type of current mirror

| **Feature**                             | **Simple Current Mirror**                | **Cascode Current Mirror**                    | **Wide-Swing Cascode Current Mirror**           | **Self-Biased Current Mirror**                 |
 | --------------------------------------- | ---------------------------------------- | --------------------------------------------- | ----------------------------------------------- | -------------------------------------------
  --- |
 | **Output Resistance (r<sub>out</sub>)** | Low   | Very High  | Very High    | High                   |
 | **Accuracy (Current Matching)**         | Low–Medium | High                                          | High                                            | Medium–High                                     |
 | **Voltage Headroom Requirement**        | Low (\~V<sub>DS(sat)</sub>)              | High (\~2 × V<sub>DS(sat)</sub>)              | Medium (\~V<sub>DS(sat)</sub> + V<sub>ov</sub>) | Medium–High (depends on topology)               |
 | **Output Voltage Swing**                | Large (good swing)                       | Small (limited due to cascoding)              | Large (improved over normal cascode)            | Medium                                          |
 | **Channel Length Modulation Effect**    | High (bad)                               | Very Low (good)                               | Very Low (good)                                 | Low–Medium                                      |
 | **Power Consumption**                   | Low                                      | Medium                                        | Medium                                          | Medium                                          |
 | **Design Complexity**                   | Very Low                                 | Medium                                        | Medium–High                                     | High                                            |
 | **Best Use Case**                       | Simple biasing, low supply voltage       | High-accuracy bias, high supply voltage       | High-accuracy bias with better swing            | On-chip bias network without external reference |


## 🔷 Comparison of Current Mirrors

 | Type | Advantages | Disadvantages |
  |------|------------|---------------|
  | **Simple Current Mirror** | - Very simple design<br>- Requires minimum components (2 matched transistors)<br>- Low area and power consumption | - Low output resistance (poor current matching for varying VOUT)<br>- Sensitive to channel length                 modulation<br>- Accuracy depends heavily on device matching |
  | **Cascode Current Mirror** | - Very high output resistance → better current matching<br>- Reduced channel length modulation effect<br>- Improved accuracy over simple mirror | - Requires higher voltage headroom (~2 × VDS(sat))<br>- Slightly more complex       design (4 transistors)<br>- Larger area |
  | **Wide-Swing Cascode Current Mirror** | - High output resistance like cascode<br>- Allows larger output voltage swing compared to standard cascode<br>- Better for low supply voltage than standard cascode | - Still more complex than simple mirror<br>-         Requires careful biasing for correct operation<br>- Voltage headroom still higher than simple mirror (but less than normal cascode) |
  | **Self-Biased Current Mirror** | - No need for an external bias voltage (bias generated internally)<br>- Compact bias network for multiple mirrors<br>- Good matching due to internal reference sharing | - More complex circuit than basic mirror<br>- Output     resistance depends on internal bias design<br>- Less flexible if different bias currents are needed in different parts of the circuit |
### Simple Current Mirror

### AC Analysis

```
*********************** Simple Current Mirror **********************
******************************* DC ANALYSIS **************************
************ Date : 25/11/2025, Designer: Chandan Shaw, Silicon University Bhubaneswar ************

.title Simple Current Mirror Using N_Channel MOSFET

.lib /home/chandanvlsi/share/pdk/sky130A/libs.tech/ngspice/sky130.lib.spice tt

.global gnd gnd
.temp 27

xmn1 Gn Gn gnd gnd sky130_fd_pr__nfet_01v8 w=5 l=2 m=4
xmn2 Dn2 Gn gnd gnd sky130_fd_pr__nfet_01v8 w=5 l=2 m=4
***If we change l value suppose l=0.5 and m=4 then the output resistance is more and performance is better and more accurate and also the change of V(Gn) Voltage and input resistance will also change and deviation is more
Iin Cn1 Gn dc 100u
Vout out gnd dc 0.9

Vcm1 vdd Cn1 dc 0
Vcm2 Out Dn2 dc 0

vsup vdd gnd dc 1.8 ac 1 sin(1.438 1m 100k)
.ac dec 20 1 1G

.control
run
set color0=white
plot v(out)
plot i(Vcm1) i(Vcm2)
plot ph((out)*180/pi)
.end
.endc
```

### DC Analysis

```
*********************** Simple Current Mirror Using N-Channel MOSFET **********************
******************************* DC ANALYSIS **************************
*********************** Date : 25/11/2025, Designer: Chandan Shaw , Silicon University  ****************************

.title Simple Current Mirror Using N_Channel MOSFET

.lib /home/chandanvlsi/share/pdk/sky130A/libs.tech/ngspice/sky130.lib.spice tt

.global gnd gnd
.temp 27

xmn1 Gn Gn gnd gnd sky130_fd_pr__nfet_01v8 w=5 l=2 m=4
xmn2 Dn2 Gn gnd gnd sky130_fd_pr__nfet_01v8 w=5 l=2 m=4

Iin Cn1 Gn dc 100u
Vout out gnd dc 0.9
** Voltage Source
Vcm1 vdd Cn1 dc 0
Vcm2 Out Dn2 dc 0
** Supply Voltage
vsup vdd gnd dc 1.8 ac 1 sin(1.438 1m 100k)
** Simulation Command
.dc Vout 0 1.8 0.01

.control
run
set color0=white
plot i(Vcm1) i(Vcm2)
plot deriv(i(Vcm2))
plot v(Gn)
*plot ph((out)*180/pi)
.end
.endc
```

### DC Analysis

```
*********************** Simple Current Mirror **********************
******************************* DC ANALYSIS **************************
*********************** Date : 25/11/2025, Designer: Chandan Shaw  ****************************

.title Simple Current Mirror Using N_Channel MOSFET
.lib /home/chandanvlsi/share/pdk/sky130A/libs.tech/ngspice/sky130.lib.spice tt

.global gnd gnd
.temp 27

xmn1 Gn Gn gnd gnd sky130_fd_pr__nfet_01v8 w=5 l=2 m=4
xmn2 Dn2 Gn gnd gnd sky130_fd_pr__nfet_01v8 w=5 l=2 m=4
Iin Cn1 Gn dc 100u
Vout out gnd dc 0.835

Vcm1 vdd Cn1 dc 0
Vcm2 Out Dn2 dc 0

vsup vdd gnd dc 1.8
*.dc Vout 0 1.8 0.01
.dc Iin 20u 200u 1u

.control
run
set color0=white
*plot i(Vcm1) i(Vcm2)
*plot deriv(i(Vcm2))
***Gate Voltage***
plot v(Gn)

plot i(Vcm1) i(Vcm2)
plot deriv(v(Gn))
.end
.endc
```

### DC Analysis

```
*********************** Simple Current Mirror **********************
******************************* DC ANALYSIS **************************
*********************** Date : 25/11/2025, Designer: Chandan Shaw  ****************************

.title Simple Current Mirror Using N_Channel MOSFET

.lib /home/chandanvlsi/share/pdk/sky130A/libs.tech/ngspice/sky130.lib.spice tt
.global gnd gnd
.temp 27

xmn1 Gn Gn gnd gnd sky130_fd_pr__nfet_01v8 w=5 l=2 m=4
xmn2 Dn2 Gn gnd gnd sky130_fd_pr__nfet_01v8 w=5 l=2 m=4
Iin Cn1 Gn dc 100u
Vout out gnd dc 0.835
Vcm1 vdd Cn1 dc 0
Vcm2 Out Dn2 dc 0
vsup vdd gnd dc 1.8

.dc Vout 0 1.8 0.01
*.dc Iin 20u 200u 1u

.control
run
set color0=white
plot i(Vcm1) i(Vcm2)
plot 1/deriv(i(Vcm2))
***Gate Voltage***
plot v(Gn)
.end
.endc
```

## Cascode Current Mirror
### DC Analysis

```
*********************** NMOS Cascode Current Mirror **********************
******************************* DC ANALYSIS **************************
*********************** Date : 25/11/2025, Designer: Chandan Shaw  ****************************

.title Cascode Current Mirror Using N_Channel MOSFET

.lib /home/chandanvlsi/share/pdk/sky130A/libs.tech/ngspice/sky130.lib.spice tt

.global gnd gnd
.temp 27

xmn1 Gn Gn gnd gnd sky130_fd_pr__nfet_01v8 w=5 l=2 m=4
xmn2 Dn2 Gn gnd gnd sky130_fd_pr__nfet_01v8 w=5 l=2 m=4
xmn3 Dn3 Dn3 Gn gnd sky130_fd_pr__nfet_01v8 w=5 l=2 m=4
xmn4 Dn4 Dn3 Dn2 gnd sky130_fd_pr__nfet_01v8 w=5 l=2 m=4

Iin Cn1 Dn3 dc 100u
Vout Out gnd dc 0.84

Vcm1 vdd Cn1 dc 0
Vcm2 Out Dn4 dc 0

vsup vdd gnd dc 1.8

.dc Vout 0 1.8 0.01
*.dc Iin 20u 200u 1u

.control
run
set color0=white
plot i(Vcm1) i(Vcm2)
plot 1/deriv(i(Vcm2))
 
.end
.endc
```
![Diagram](https://github.com/Chandan-Shaw/Characterization/blob/main/Cascode_Current_Mirror_dc_Simulation.png)
![Diagram](https://github.com/Chandan-Shaw/Characterization/blob/main/Cascode_Current_Mirror_dc1_Simulation.png)
![Diagram](https://github.com/Chandan-Shaw/Characterization/blob/main/Cascode_Current_Mirror_dc2_Simulation.png)

### DC Analysis

```
*********************** NMOS Cascode Current Mirror **********************
******************************* DC ANALYSIS **************************
*********************** Date : 25/11/2025, Designer: Chandan Shaw  ****************************

.title Cascode Current Mirror Using N_Channel MOSFET
.lib /home/chandanvlsi/share/pdk/sky130A/libs.tech/ngspice/sky130.lib.spice tt
.global gnd gnd
.temp 27

xmn1 Gn Gn gnd gnd sky130_fd_pr__nfet_01v8 w=5 l=2 m=4
xmn2 Dn2 Gn gnd gnd sky130_fd_pr__nfet_01v8 w=5 l=2 m=4
xmn3 Dn3 Dn3 Gn gnd sky130_fd_pr__nfet_01v8 w=5 l=2 m=4
xmn4 Dn4 Dn3 Dn2 gnd sky130_fd_pr__nfet_01v8 w=5 l=2 m=4

Iin Cn1 Dn3 dc 100u
Vout Out gnd dc 0.84

Vcm1 vdd Cn1 dc 0
Vcm2 Out Dn4 dc 0

vsup vdd gnd dc 1.8

.dc Iin 0 200u 1u

.control
run
set color0=white
plot i(Vcm1) i(Vcm2)
plot v(Gn) v(Dn3) v(Dn2)
plot deriv(v(Gn))
plot deriv(v(Dn3))
.end
.endc
```
![Diagram](https://github.com/Chandan-Shaw/Characterization/blob/main/Cascode_Current_dc_Simulation.png)
![Diagram](https://github.com/Chandan-Shaw/Characterization/blob/main/Cascode_Current_dc1_Simulation.png)
![Diagram](https://github.com/Chandan-Shaw/Characterization/blob/main/Cascode_Current_dc2_Simulation.png)
![Diagram](https://github.com/Chandan-Shaw/Characterization/blob/main/Cascode_Current_dc3_Simulation.png)

## Wide Swing Cascode Current Mirror

### DC Analysis
 ```
*************************** Wide Swing Cascode Current Mirror Using NMOS ****************************
************************************************* DC ANALYSIS ****************************************
************ Date: 11/01/2026 , Designer: Chandan Shaw , Silicon University Bhubaneshwar *************

.title Wide Swing Cascode Current Mirror Using N Channel MOSFET
.lib /home/chandanvlsi/share/pdk/sky130A/libs.tech/ngspice/sky130.lib.spice tt

.global gnd
.temp 27

xmn1 Dn1 Gn gnd gnd sky130_fd_pr__nfet_01v8 w=5 l=2 m=4
xmn2 Dn2 Gn gnd gnd sky130_fd_pr__nfet_01v8 w=5 l=2 m=4
xmn3 Gn Dn5 Dn1 gnd sky130_fd_pr__nfet_01v8 w=5 l=2 m=4
xmn4 Dn4 Dn5 Dn2 gnd sky130_fd_pr__nfet_01v8 w=5 l=2 m=4
xmn5 Dn5 Dn5 gnd gnd sky130_fd_pr__nfet_01v8 w=5 l=8 m=4

Iin1 Cn1 Gn dc 100u
Iin2 Cn2 Dn5 dc 100u

Vout Out gnd dc 0.84

Vcm1 vdd Cn1 dc 0
Vcm2 Out Dn4 dc 0
Vcm3 vdd Cn2 dc 0

Vsup vdd gnd dc 1.8

.dc Vout 0 1.8 0.01
.control
run
set color0=white
plot i(Vcm1) i(Vcm2) i(Vcm3)
plot 1/deriv(i(Vcm2))
plot v(Dn5) v(Dn1) v(Gn) v(Dn2)
.end
.endc
```
![Diagram](https://github.com/Chandan-Shaw/Characterization/blob/main/WS_Cascode_dc_Simulation.png)
![Diagram](https://github.com/Chandan-Shaw/Characterization/blob/main/WS_Cascode_dc1_Simulation.png)
![Diagram](https://github.com/Chandan-Shaw/Characterization/blob/main/WS_Cascode_dc2_Simulation.png)

## Self Baised Wide Swing Cascode Current Mirror

### DC Analysis
 
```
************************ Self Baised Wide Swing Cascode Current Mirror Using NMOS *********************************
************************ DC ANALYSIS ***************************
************* Date: 11/01/2026 , Designer: Chandan Shaw , Silicon University Bhubaneshwar ********************

.title Self Baised Wide Swing Cascode Current Mirror Using N Channel MOSFET
.lib /home/chandanvlsi/share/pdk/sky130A/libs.tech/ngspice/sky130.lib.spice tt
.global gnd
.temp 27

xmn1 Dn1 Gn gnd gnd sky130_fd_pr__nfet_01v8 w=5 l=2 m=4
xmn2 Dn2 Gn gnd gnd sky130_fd_pr__nfet_01v8 w=5 l=2 m=4
xmn3 Gn Rt1 Dn1 gnd sky130_fd_pr__nfet_01v8 w=5 l=2 m=4
xmn4 Dn4 Rt1 Dn2 gnd sky130_fd_pr__nfet_01v8 w=5 l=2 m=4
Ron Rt1 Gn 3k
Iin Cn1 Rt1 dc 100u

Vout Out gnd dc 0.84
Vcm1 vdd Cn1 dc 0
Vcm2 Out Dn4 dc 0

Vsup vdd gnd dc 1.8
.dc Vout 0 1.8 0.01

.control
run
set color0=white
plot i(Vcm1) i(Vcm2)
plot 1/deriv(i(Vcm2))
plot v(Dn1) v(Gn) v(Dn2) v(Rt1)
.end
.endc
```
![Diagram](https://github.com/Chandan-Shaw/Characterization/blob/main/SB_WS_Cascode_dc_Simulation.png)
![Diagram](https://github.com/Chandan-Shaw/Characterization/blob/main/SB_WS_Cascode_dc1_Simulation.png)
![Diagram](https://github.com/Chandan-Shaw/Characterization/blob/main/SB_WS_Cascode_dc2_Simulation.png)

## 7. Single Stage Amplifiers
- A single-stage amplifier is the simplest form of amplifier — it uses just one active device (like a BJT, MOSFET, or JFET) along with biasing and load components to amplify a weak input signal into a stronger output signal.
- The term single stage means the signal passes through only one amplifying device before reaching the output.

### Advantages
- Simple design — easy to understand and build.
- Low cost — fewer components.
- Foundation for multi-stage amplifiers.

### Disadvantages
- Limited gain — can’t amplify very weak signals to large values in one stage.
- Lower input/output impedance control — may not match all sources/loads.
- Limited bandwidth — affected by transistor parasitics and load capacitance.
 - Noisy — more susceptible to noise compared to differential stages
 
## 7.1 Common Source Amplifier using NMOS

### Netlist Code Of Common Source Amplifier using NMOS With Resistive Load
**AC Analysis**
```
************* Common Source Amplifier With N-Channel MOSFET and Resistive Load ****************
***************************************** AC ANALYSIS *****************************************
*********************** Date : 30/10/2025, Designer: Chandan Shaw  ****************************

.title Common Source Amplifier With N-Channel MOSFET and Resistive Load

.lib /home/chandanvlsi/share/pdk/sky130A/libs.tech/ngspice/sky130.lib.spice tt

.global gnd
.temp 27

xmn1 out in gnd gnd sky130_fd_pr__nfet_01v8 w=7 l=2 m=2
Rd avdd out 8k
Cl out gnd 10p

vsup avdd gnd dc 1.8
Vin in gnd dc 0.9 ac 1 sin(0.9 1m 100k)

.ac dec 20 1 1G

.control
run
set color0=white
plot v(in) v(out)
plot ph(v(out)/v(in))
.end
.endc
```

![Diagram](https://github.com/Chandan-Shaw/Characterization/blob/main/CS_Amp_resi_Load_ac_Simulation.png)
![Diagram](https://github.com/Chandan-Shaw/Characterization/blob/main/CS_Amp_resis_Load_ac_Simulation.png)

### Netlist Code Of Common Source Amplifier Using NMOS and PMOS Current Source Load

**DC Analysis**
```
****************** Common Source Amplifier With N-Channel MOSFET and Current Source Load ***************
************************************ DC Analysis *************************************
**************** Date: 30/11/2025, Designer: Chandan Shaw, Silicon University Bhubaneswar **************

.title CS Amplifier With NMOS Driver and PMOS Current Source Load

.lib /home/chandanvlsi/share/pdk/sky130A/libs.tech/ngspice/sky130.lib.spice tt

.global gnd vdd
.temp 27

xmn1 out in gnd gnd sky130_fd_pr__nfet_01v8 w=7 l=2 m=2
xmn2 Dn2 Gn2 gnd gnd sky130_fd_pr__nfet_01v8 w=7 l=2 m=2
xmp1 Dp1 Dp2 vdd vdd sky130_fd_pr__pfet_01v8_lvt w=7 l=2 m=6
xmp2 Dp2 Dp2 vdd vdd sky130_fd_pr__pfet_01v8_lvt w=7 l=2 m=6

Vcm1 Dp1 out dc 0
Vcm2 Dp2 Dn2 dc 0
Cl out gnd 10p

vsup vdd gnd dc 1.8
Vin in gnd dc 0.9 ac 1 sin(0.9 1m 100k)
Vbn1 Gn2 gnd dc 0.9

.dc Vin 0 1.8 0.01

.control
run
set color0=white
plot v(out) v(Dp2)
plot i(Vcm1) i(Vcm2)
print abs(v(Dp2))
.end
.endc
```
![Diagram](https://github.com/Chandan-Shaw/Characterization/blob/main/CS_Amp_i_load_dc_Simulation.png)
![Diagram](https://github.com/Chandan-Shaw/Characterization/blob/main/CS_Amp_i_load_dc_Simulations.png)

### Netlist Code Of Common Source Amplifier Using NMOS and PMOS Current Source Load

**AC Analysis**
```
****************** Common Source Amplifier With N-Channel MOSFET and PMOS Current Source Load ***************
************************************ AC Analysis *************************************
**************** Date: 30/11/2025, Designer: Chandan Shaw, Silicon University Bhubaneswar **************

.title CS Amplifier With NMOS Driver and PMOS Current Source Load

.lib /home/chandanvlsi/share/pdk/sky130A/libs.tech/ngspice/sky130.lib.spice tt

.global gnd vdd
.temp 27

xmn1 out in gnd gnd sky130_fd_pr__nfet_01v8 w=7 l=2 m=2
xmn2 Dn2 Gn2 gnd gnd sky130_fd_pr__nfet_01v8 w=7 l=2 m=2
xmp1 Dp1 Dp2 vdd vdd sky130_fd_pr__pfet_01v8_lvt w=7 l=2 m=6
xmp2 Dp2 Dp2 vdd vdd sky130_fd_pr__pfet_01v8_lvt w=7 l=2 m=6

Vcm1 Dp1 out dc 0
Vcm2 Dp2 Dn2 dc 0
Cl out gnd 10p

vsup vdd gnd dc 1.8
Vin in gnd dc 0.9 ac 1 sin(0.9 1m 100k)
Vbn1 Gn2 gnd dc 0.9

.ac dec 20 1 1G

.control
run
set color0=white
plot v(in) abs(v(out)) xlabel 'Frequency' ylabel 'Gain(mag)'
plot abs(v(out))/v(in) xlabel 'Frequency' ylabel 'Gain(mag)'
plot vdb(out)          xlabel 'Frequency' ylabel 'Gain(mag)'
plot ph(out)*(180/pi)  xlabel 'Frequency' ylabel 'Gain(mag)'

print vdb(out)
.end
.endc
```
![Diagram](https://github.com/Chandan-Shaw/Characterization/blob/main/CS_Amp_i_load_ac_Simulation.png)
![Diagram](https://github.com/Chandan-Shaw/Characterization/blob/main/CS_Amp_i_load_ac1_Simulation.png)
![Diagram](https://github.com/Chandan-Shaw/Characterization/blob/main/CS_Amp_i_load_ac2_Simulation.png)
![Diagram](https://github.com/Chandan-Shaw/Characterization/blob/main/CS_Amp_i_load_ac3_Simulation.png)

### Netlist Code Of Common Source Amplifier Using NMOS and PMOS Current Source Load

**Transient Analysis**

```
****************** Common Source Amplifier With N-Channel MOSFET and Current Source Load ***************
************************************ TRANSIENT Analysis *************************************
**************** Date: 30/11/2025, Designer: Chandan Shaw, Silicon University Bhubaneswar **************

.title CS Amplifier With NMOS Driver and PMOS Current Source Load

.lib /home/chandanvlsi/share/pdk/sky130A/libs.tech/ngspice/sky130.lib.spice tt

.global gnd vdd
.temp 27

xmn1 out in gnd gnd sky130_fd_pr__nfet_01v8 w=7 l=2 m=2
xmn2 Dn2 Gn2 gnd gnd sky130_fd_pr__nfet_01v8 w=7 l=2 m=2
xmp1 Dp1 Dp2 vdd vdd sky130_fd_pr__pfet_01v8_lvt w=7 l=2 m=6
xmp2 Dp2 Dp2 vdd vdd sky130_fd_pr__pfet_01v8_lvt w=7 l=2 m=6

Vcm1 Dp1 out dc 0
Vcm2 Dp2 Dn2 dc 0
Cl out gnd 10p

vsup vdd gnd dc 1.8
Vin in gnd dc 0.9 ac 1 sin(0.9 1m 10k)
Vbn1 Gn2 gnd dc 0.9

.tran 1n 1000u

.control
run
set color0=white
plot v(in)
plot v(out)
.end
.endc
```

## 7.2 Common Gate Amplifier using NMOS

### Netlist Code Of Common Gate Amplifier using NMOS With Resistive Load
**DC Analysis**
```
********************** Common GATE Amplifier with MOSFET load ************
******************************* DC ANALYSIS ********************************
****************************** Date : 28/10/2025,  Designer:  Chandan Shaw *******************

.title Common Gate Amplifier With MOSFET Load

.lib /home/chandanvlsi/share/pdk/sky130A/libs.tech/ngspice/sky130.lib.spice tt

.global gnd
.temp 27

xm1 out Gn1 Sn1 gnd sky130_fd_pr__nfet_01v8 w=5 l=2 m=4
Rd Rt1 out 8k
Cl out gnd 10p
Vcm vdd Rt1 dc 0


Vsup vdd gnd dc 1.8
Vgs Gn1 gnd dc 1.2
Vss Sn1 gnd dc 0.2 ac 1 sin(0.2 10m 1k)

.dc Vgs 0 1.8 0.01
*.ac dec 10 1 1G
*.tran 20u 1n

.control
run
set color0=white
plot i(Vcm)
plot v(Sn1) v(out)
plot deriv(out)
*plot db(out)
*plot ph((out)*180/pi)
.end
.endc
```

### Netlist Code Of Common Gate Amplifier using NMOS With Resistive Load
**AC Analysis**
```
********************** Common GATE Amplifier with MOSFET load ************
******************************* AC ANALYSIS ********************************
****************************** Date : 28/10/2025,  Designer:  Chandan Shaw *******************

.title Common Gate Amplifier With MOSFET Load

.lib /home/chandanvlsi/share/pdk/sky130A/libs.tech/ngspice/sky130.lib.spice tt

.global gnd
.temp 27

xm1 out Gn1 Sn1 gnd sky130_fd_pr__nfet_01v8 w=5 l=2 m=4
Rd Rt1 out 8k
Cl out gnd 10p
Vcm vdd Rt1 dc 0

Vsup vdd gnd dc 1.8
Vgs Gn1 gnd dc 1.08104
Vss Sn1 gnd dc 0.2 ac 1 sin(0.2 10m 1k)

.ac dec 10 1 1G

.control
run
set color0=white
plot i(Vss) i(Vcm)
plot v(Sn1) v(Gn1) v(out)
plot db(out) db(Sn1)
plot ph(out)*(180/pi)
.end
.endc
```
### Netlist Code Of Common Gate Amplifier using NMOS With Resistive Load
**Transient Analysis**
```
********************** Common GATE Amplifier with MOSFET load ************
******************************* Transient ANALYSIS ********************************
****************************** Date : 28/10/2025,  Designer:  Chandan Shaw *******************

.title Common Gate Amplifier With MOSFET Load
.lib /home/chandanvlsi/share/pdk/sky130A/libs.tech/ngspice/sky130.lib.spice tt

.global gnd
.temp 27

xm1 out Gn1 Sn1 gnd sky130_fd_pr__nfet_01v8 w=5 l=2 m=4
Rd Rt1 out 8k
Cl out gnd 10p
Vcm vdd Rt1 dc 0

Vsup vdd gnd dc 1.8
Vgs Gn1 gnd dc 1.0813
Vss Sn1 gnd dc 0.2 ac 1 sin(0.2 10m 1k)

.tran 1n 10m

.control
run
set color0=white
plot v(Sn1)
plot v(out)
.end
.endc
```

## 7.3 Common Drain Amplifier using NMOS

### Netlist Code Of Common Drain Amplifier using NMOS With MOSFET Load
**DC Analysis**

```
********************** Common Drain Amplifier with MOSFET load ************
******************************* DC ANALYSIS ********************************
****************************** Date : 28/10/2025,  Designer:  Chandan Shaw *******************

.title Common Drain Amplifier With MOSFET Load

.lib /home/chandanvlsi/share/pdk/sky130A/libs.tech/ngspice/sky130.lib.spice tt

.global gnd gnd
.temp 27

xm1 Dn1 in out gnd sky130_fd_pr__nfet_01v8 w=5 l=2 m=4
xm2 out Gn2 gnd gnd sky130_fd_pr__nfet_01v8 w=5 l=2 m=4
*Rl Out Gnd 9
Vcm vdd Dn1 dc 0
Cl out gnd 10p

Vsup vdd gnd dc 1.8
Vbias Gn2 gnd dc 0.85
Vin in gnd dc 1.5 ac 1 sin(1.438 1m 100k)

*.dc Vbn1 0 1.8 0.01
.dc Vin 0 1.8 0.01

.control
run
set color0=white
plot v(out)
plot i(Vcm)
plot deriv(v(out))
.end
.endc
```

### Netlist Code Of Common Drain Amplifier using NMOS With MOSFET Load
**AC Analysis**
```
********************** Common Drain Amplifier with MOSFET load *************
******************************* AC ANALYSIS ********************************
***************** Date : 28/10/2025,  Designer:  Chandan Shaw **************

.title Common Drain Amplifier With MOSFET Load

.lib /home/chandanvlsi/share/pdk/sky130A/libs.tech/ngspice/sky130.lib.spice tt

.global gnd gnd
.temp 27

xm1 Dn1 in out gnd sky130_fd_pr__nfet_01v8 w=5 l=2 m=4
xm2 out Gn2 gnd gnd sky130_fd_pr__nfet_01v8 w=5 l=2 m=4
Vcm vdd Dn1 dc 0
Cl out gnd 10p

Vsup vdd gnd dc 1.8
Vbias Gn2 gnd dc 0.85
Vin in gnd dc 1.14 ac 1 sin(1.438 1m 100k)

.ac dec 10 1 1G

.control
run
set color0=white
plot v(out)
plot i(Vcm)
plot ph((out)*180/pi)
*plot deriv(v(out))
.end
.endc
```

### Netlist Code Of Common Drain Amplifier using NMOS With MOSFET Load
**Transient Analysis**
```
********************** Common Drain Amplifier with MOSFET load ************
******************************* DC ANALYSIS ********************************
****************************** Date : 28/10/2025,  Designer:  Chandan Shaw *******************

.title Common Drain Amplifier With MOSFET Load

.lib /home/chandanvlsi/share/pdk/sky130A/libs.tech/ngspice/sky130.lib.spice tt

.global gnd gnd
.temp 27

xm1 Dn1 in out gnd sky130_fd_pr__nfet_01v8 w=5 l=2 m=4
xm2 out Gn2 gnd gnd sky130_fd_pr__nfet_01v8 w=5 l=2 m=4
Vcm vdd Dn1 dc 0
Cl out gnd 10p

Vsup vdd gnd dc 1.8
Vbias Gn2 gnd dc 0.85
Vin in gnd dc 1.5 ac 1 sin(1.438 1m 100k)

.tran 1n 10m

.control
run
set color0=white
plot v(out)
plot i(Vcm)
plot deriv(v(out))
.end
.endc
```

### Netlist Code Of Common Drain Amplifier using NMOS With Resistive Load
**DC Analysis**
```
******************** Common Drain Amplifier with resistive load ************
******************************* DC ANALYSIS ********************************
*************** Date : 28/10/2025,  Designer:  Chandan Shaw ****************

.title Common Drain Amplifier With Resistive Load

.lib /home/chandanvlsi/share/pdk/sky130A/libs.tech/ngspice/sky130.lib.spice tt

.global gnd gnd
.temp 27

xm1 Dn1 in out gnd sky130_fd_pr__nfet_01v8 w=5 l=2 m=4
Rs out gnd 5k
*Rl Out Gnd 9
Vcm vdd Dn1 dc 0
Cl out gnd 10p

vsup vdd gnd dc 1.8
Vin in gnd dc 1.5 ac 1 sin(1.438 1m 100k)

*.dc Vbn1 0 1.8 0.01
.dc Vin 0 1.8 0.01

.control
run
set color0=white
plot v(out)
plot i(Vcm)
plot deriv(v(out))
.end
.endc
```

### Netlist Code Of Common Drain Amplifier using NMOS With Resistive Load
**AC Analysis**

```
********************* Common Drain Amplifier with resistive load ************
****************************** AC ANALYSIS ********************************
***************** Date : 28/10/2025,  Designer:  Chandan Shaw ****************

.title Common Drain Amplifier With Resistive Load

.lib /home/chandanvlsi/share/pdk/sky130A/libs.tech/ngspice/sky130.lib.spice tt

.global gnd gnd
.temp 27

xm1 Dn1 in out gnd sky130_fd_pr__nfet_01v8 w=5 l=2 m=4
Rs out gnd 5k
*Rl Out Gnd 9
Vcm vdd Dn1 dc 0
Cl out gnd 10p

vsup vdd gnd dc 1.8
Vin in gnd dc 1.5 ac 1 sin(1.438 1m 100k)

.ac dec 10 1 10G

.control
run
set color0=white
plot v(out)
plot ph(out)*(180/pi)
.end
.endc
```

### Netlist Code Of Common Drain Amplifier using NMOS With Resistive Load
**Transient Analysis**
```
********************** Common Drain Amplifier with resistive load ************
******************************* TRAN ANALYSIS ********************************
****************************** Date : 28/10/2025,  Designer:  Chandan Shaw *******************

.title Common Drain Amplifier With Resistive Load

.lib /home/chandanvlsi/share/pdk/sky130A/libs.tech/ngspice/sky130.lib.spice tt

.global gnd gnd
.temp 27

xm1 Dn1 in out gnd sky130_fd_pr__nfet_01v8 w=5 l=2 m=4
Rs out gnd 5k
*Rl Out Gnd 9
Vcm vdd Dn1 dc 0
Cl out gnd 10p

vsup vdd gnd dc 1.8
Vin in gnd dc 1.5 ac 1 sin(1.438 300m 100k)

.tran 10p 100u

.control
run
set color0=white
plot v(in) v(out)

.end
.endc
```

## 8. Cascode Amplifier

## 8.1 Cascode Amplifier using NMOS

### Netlist Code Of Cascode Amplifier using NMOS With MOSFET Load
**DC Analysis**
```
********************** Cascode Amplifier With MOS Load *******************
******************************* DC ANALYSIS ******************************
**************** Date : 30/10/2025, Designer: Chandan Shaw  **************

.title Cascode Amplifier Wirh NMOS Driver and Resistive Load

.lib /home/chandanvlsi/share/pdk/sky130A/libs.tech/ngspice/sky130.lib.spice tt

.global gnd vdd
.temp 27

xmn1 Dn1 in gnd gnd sky130_fd_pr__nfet_01v8 w=5 l=2 m=4
xmn2 Out Gn2 Dn1 gnd sky130_fd_pr__nfet_01v8 w=5 l=2 m=4
xmp1 Out Gp1 Ds1 vdd sky130_fd_pr__pfet_01v8_lvt w=5 l=2 m=4
Rd Rt1 Out 8k
Vcm vdd Rt1 dc 0
Cl out gnd 10p

vsup vdd gnd dc 1.8
vb2 Gn2 gnd dc 1.3283
Vin in gnd dc 0.84 ac 1 sin(0.9 1m 100k)

.dc Vin 0 1.8 0.01

.control
run
set color0=white
plot v(out) v(Dn1)
plot v(Out)
plot v(Dn1)
plot i(Vcm)
plot deriv(v(out))
.end
.endc
```
### Netlist Code Of Cascode Amplifier using NMOS With MOSFET Load
**AC Analysis**

```
****************** Cascode Amplifier With Resistive Load *******************
******************************* AC ANALYSIS ********************************
****************** DAte : 30/10/2025, Designer: Chandan Shaw  **************

.title Cascode Amplifier Wirh NMOS Driver and Resistive Load

.lib /home/chandanvlsi/share/pdk/sky130A/libs.tech/ngspice/sky130.lib.spice tt

.global gnd vdd
.temp 27

xmn1 Dn1 in gnd gnd sky130_fd_pr__nfet_01v8 w=5 l=2 m=4
xmn2 Out Gn2 Dn1 gnd sky130_fd_pr__nfet_01v8 w=5 l=2 m=4
xmn2 Out Gp1 Ds1 vdd sky130_fd_pr__nfet_01v8_lvt w=5 l=2 m=14
Vcm vdd Rt1 dc 0
Cl out gnd 10p

vsup vdd gnd dc 1.8
Vbn2 Gn2 gnd dc 1.3283
Vbp1 Gp1 gnd dc 0.998
Vin in gnd dc 0.84 ac 1 sin(0.9 1m 100k)

.ac dec 10 1 1G

.control
run
set color0=white
plot db(Out)
plot ph(out)*(180/pi)
plot v(Out)*-1
plot v(Dn1)*-1
.end
.endc
```

### Netlist Code Of Cascode Amplifier using NMOS With MOSFET Load
**Transient Analysis**

```
****************** Cascode Amplifier With Resistive Load *******************
******************************* TRANSIENT ANALYSIS *************************
****************** Date : 30/10/2025, Designer: Chandan Shaw  **************

.title Cascode Amplifier With NMOS Driver and Resistive Load

.lib /home/chandanvlsi/share/pdk/sky130A/libs.tech/ngspice/sky130.lib.spice tt

.global gnd vdd
.temp 27

xmn1 Dn1 in gnd gnd sky130_fd_pr__nfet_01v8 w=5 l=2 m=4
xmn2 Out Gn2 Dn1 gnd sky130_fd_pr__nfet_01v8 w=5 l=2 m=4
xmp1 Out Gp1 Ds1 vdd sky130_fd_pr__nfet_01v8_lvt w=5 l=2 m=14
Vcm vdd Ds1 dc 0
Cl out gnd 10p

vsup vdd gnd dc 1.8
vb2 Gn2 gnd dc 1.3283
vbp1 Gp1 gnd dc 0.998
Vin in gnd dc 0.84 ac 1 sin(0.84 1m 1k)

.tran 1u 10m

.control
run
set color0=white
plot v(in)
plot v(Out)
.end
.endc
```

## 9. Differential Amplifier

- A differential amplifier is a type of electronic amplifier that amplifies the difference between two input signals while rejecting any voltage common to both inputs (called common-mode signals).
- It’s one of the most fundamental building blocks in analog and mixed-signal circuits.

### Advantages
- High CMRR
- Better stability

### Disadvantages
- Requires matched components for ideal operation.
- More complex biasing compared to single-ended amplifiers.

### 9.1 Differential Amplifier using NMOS

### Netlist Code Of Differential Amplifier 
**DC Analysis**
```
****************** Differntial Amplifier Using NMOS ************************
******************************* DC ANALYSIS ********************************
****************** Date : 26/11/2025, Designer: Chandan Shaw  **************

.title DC Analysis Of Differntial Amplifier Using NMOS
.lib /home/chandanvlsi/share/pdk/sky130A/libs.tech/ngspice/sky130.lib.spice tt
.temp 25

XM1 g1  g1 d  d sky130_fd_pr__pfet_01v8_lvt  L=8 W=7 m=10
XM2 n1  g1 d  d sky130_fd_pr__pfet_01v8_lvt  L=8 W=7 m=10
XM3 g1  n3  n2  0 sky130_fd_pr__nfet_01v8_lvt  L=0.5 W=7
XM4 n1  n3  n7  0 sky130_fd_pr__nfet_01v8_lvt  L=0.5 W=7
XM5 n4  n4  0  0 sky130_fd_pr__nfet_01v8_lvt  L=4 W=5 m=10
XM6 n8  n4  0  0 sky130_fd_pr__nfet_01v8_lvt  L=4 W=5 m=10

Vn  n3 0 1.25
Iref 0 n4  50u
C1 n1 0 500f
Vdd d 0 1.8
V1 n2 n6 0
V2 n7 n6 0
`V3 n6 n8 0

.op
.control
run
*dc Vn 0 1.8 0.01
*plot v(n1)
print v(n2)
print v(g1)
print v(n1)
print i(vdd)
print i(V1)
print i(V2)
print i(v3)
.endc
.end
```

### Netlist Code Of Differential Amplifier 
**AC Analysis**

```
****************** Differntial Amplifier Using NMOS ************************
******************************* AC ANALYSIS ********************************
****************** Date : 26/11/2025, Designer: Chandan Shaw  **************

.title AC Analysis Of Differntial Amplifier Using NMOS
.lib /home/chandanvlsi/share/pdk/sky130A/libs.tech/ngspice/sky130.lib.spice tt
.temp 25

XM1 g1  g1 d  d sky130_fd_pr__pfet_01v8_lvt  L=8 W=7 m=10
XM2 n1  g1 d  d sky130_fd_pr__pfet_01v8_lvt  L=8 W=7 m=10
XM3 g1  n3  n2  0 sky130_fd_pr__nfet_01v8_lvt  L=.5 W=7
XM4 n1  n5  n7  0 sky130_fd_pr__nfet_01v8_lvt  L=.5 W=7
XM5 n4  n4  0  0 sky130_fd_pr__nfet_01v8_lvt  L=4 W=5 m=10
XM6 n8  n4  0  0 sky130_fd_pr__nfet_01v8_lvt  L=4 W=5 m=10

Vdd d 0 1.8
Vn  n3 0 1.25 ac 0.5
Vp  n5 0 1.25 ac -0.5
Iref 0 n4  50u
C1 n1 0 500f
V1 n2 n6 0
V2 n7 n6 0
V3 n6 n8 0

.ac dec 10 1 15meg
.control
run
plot (180/3.141)*ph(n1)
plot vdb(n1)
.endc
.end
```

### Netlist Code Of Differential Amplifier 
**TRANSIENT Analysis**

```
****************** Differntial Amplifier Using NMOS ************************
**************************** TRANSIENT ANALYSIS ****************************
****************** Date : 26/11/2025, Designer: Chandan Shaw  **************

.title Transient Analysis Of Differntial Amplifier Using NMOS
.lib /home/chandanvlsi/share/pdk/sky130A/libs.tech/ngspice/sky130.lib.spice tt
.temp 25

XM1 g1  g1 d  d sky130_fd_pr__pfet_01v8_lvt  L=8 W=7 m=10
XM2 n1  g1 d  d sky130_fd_pr__pfet_01v8_lvt  L=8 W=7 m=10
XM3 g1  n3  n2  0 sky130_fd_pr__nfet_01v8_lvt  L=.5 W=7
XM4 n1  n5  n7  0 sky130_fd_pr__nfet_01v8_lvt  L=.5 W=7
XM5 n4  n4  0  0 sky130_fd_pr__nfet_01v8_lvt  L=4 W=5 m=10
XM6 n8  n4  0  0 sky130_fd_pr__nfet_01v8_lvt  L=4 W=5 m=10

Vdd d 0 1.8
Vn  n3 0 sin(1.25 5m 10k)
Vp  n5 0 sin(1.25 5m 10k 0 0 180)
Iref 0 n4  50u
C1 n1 0 500f
V1 n2 n6 0
V2 n7 n6 0
V3 n6 n8 0

.tran 0.1u 100u
.control
run
plot v(n3) v(n5)
plot v(n1)
.endc
.end
```

### 9.2 Differential Amplifier using PMOS

### Netlist Code Of Differential Amplifier 
**DC Analysis**

```
****************** Differntial Amplifier Using PMOS ************************
******************************* DC ANALYSIS ********************************
****************** Date : 26/11/2025, Designer: Chandan Shaw  **************

.title DC Analysis Of Differntial Amplifier Using PMOS
.lib /home/chandanvlsi/share/pdk/sky130A/libs.tech/ngspice/sky130.lib.spice tt
.temp 25

XM1 g1  g1 d  d sky130_fd_pr__pfet_01v8_lvt  L=8 W=7 m=10
XM2 n1  g1 d  d sky130_fd_pr__pfet_01v8_lvt  L=8 W=7 m=10
XM3 g2  i1  n2  n2  sky130_fd_pr__pfet_01v8_lvt  L=0.5 W=7 m=5
XM4 n3  i1  n2  n2  sky130_fd_pr__pfet_01v8_lvt  L=0.5 W=7 m=5
XM5 g2  g2  n4  0   sky130_fd_pr__nfet_01v8  L=4 W=2 m=5
XM6 n3  g2  n5  0   sky130_fd_pr__nfet_01v8  L=4 W=2 m=5

Vdd d 0 1.8
V1 n1 n2 0
Iref g1 0 50u
V2 n4 0 0
V3 n5 0 0
V4 i1 0 0.8
C1 n3 0 500f
.op
.control
run

*dc V4 0 1.8 0.01
*plot  v(n3)
print v(n2)
print v(g2)
print v(n3)
print i(vdd)
print i(V1)
print i(V2)
print i(v3)
.endc
.end
```

### Netlist Code Of Differential Amplifier 
**AC Analysis**

```
****************** Differntial Amplifier Using PMOS ************************
******************************* AC ANALYSIS ********************************
****************** Date : 26/11/2025, Designer: Chandan Shaw  **************

.title AC Analysis Of Differntial Amplifier Using PMOS
.lib /home/chandanvlsi/share/pdk/sky130A/libs.tech/ngspice/sky130.lib.spice tt
.temp 25

XM1 g1  g1 d  d sky130_fd_pr__pfet_01v8_lvt  L=8 W=7 m=10
XM2 n1  g1 d  d sky130_fd_pr__pfet_01v8_lvt  L=8 W=7 m=10
XM3 g2  i1  n1  n1  sky130_fd_pr__pfet_01v8_lvt  L=0.35 W=7 m=5
XM4 n3  i2  n1  n1  sky130_fd_pr__pfet_01v8_lvt  L=0.35 W=7 m=5
XM5 g2  g2  0  0 sky130_fd_pr__nfet_01v8  L=4 W=2 m=5
XM6 n3  g2  0  0 sky130_fd_pr__nfet_01v8  L=4 W=2 m=5

Vdd d 0 1.8
Iref g1 0 50u
Vn  i1 0 0.8 ac 0.5
Vp  i2 0 0.8 ac -0.5
C1 n3 0 500f

.ac dec 10 1 15meg
.control
run
let phase = (180/3.141)*ph(n3)
let gain = vdb(n3)
plot gain
plot phase
.endc
.end
```

### Netlist Code Of Differential Amplifier 
**TRANSIENT Analysis**

```
****************** Differntial Amplifier Using PMOS ************************
*************************** TRANSIENT ANALYSIS *****************************
****************** Date : 26/11/2025, Designer: Chandan Shaw  **************

.title TRANSIENT Analysis Of Differntial Amplifier Using PMOS
.lib /home/chandanvlsi/share/pdk/sky130A/libs.tech/ngspice/sky130.lib.spice tt
.temp 25

XM1 g1  g1 d  d sky130_fd_pr__pfet_01v8_lvt  L=8 W=7 m=10
XM2 n1  g1 d  d sky130_fd_pr__pfet_01v8_lvt  L=8 W=7 m=10
XM3 g2  i1  n1  n1  sky130_fd_pr__pfet_01v8_lvt  L=0.35 W=7 m=5
XM4 n3  i2  n1  n1  sky130_fd_pr__pfet_01v8_lvt  L=0.35 W=7 m=5
XM5 g2  g2  0  0 sky130_fd_pr__nfet_01v8  L=4 W=2 m=5
XM6 n3  g2  0  0 sky130_fd_pr__nfet_01v8  L=4 W=2 m=5

Vdd d 0 1.8
Vn  i1 0 sin(0.8 1m 1k)
Vp  i2 0 sin(0.8 1m 1k 0 0 180)
Iref g1 0 50u
C1 n3 0 500f

.tran 1u 1000u
.control
run
plot v(i1) v(i2)
plot v(n3)
.endc
.end
```

## 10. Fully Differential Amplifier

### Fully Differential Amplifier using NMOS

### Netlist Code Of Fully Differential Amplifier 
**DC Analysis**

```
*********************** Fully Differential Amplifier With Resistive Load **********************
******************************* DC ANALYSIS **************************
*********************** Date : 24/11/2025, Designer: Chandan Shaw  ****************************

.title Fully Differential Amplifier With Resistive Load

.lib /home/chandanvlsi/share/pdk/sky130A/libs.tech/ngspice/sky130.lib.spice tt

.global gnd gnd
.temp 27
xmn0 Dn0 Dn5 gnd gnd sky130_fd_pr__nfet_01v8 w=5 l=2 m=8
xmn6 Dn5 Dn5 gnd gnd sky130_fd_pr__nfet_01v8 w=5 l=2 m=8
xmn1 Outn In S12 gnd sky130_fd_pr__nfet_01v8 w=5 l=2 m=4
xmn2 Outp In S12 gnd sky130_fd_pr__nfet_01v8 w=5 l=2 m=4
Rld1 Sp3 Outn 8k
Rld2 Sp4 Outp 8k

Vcm0 S12 Dn0 dc 0
Vcm3 vdd Sp3 dc 0
Vcm4 vdd Sp4 dc 0
Icm5 vdd Dn5 dc 200u
Cl1 Outn gnd 20p
Cl2 Outp gnd 20p

Vsup vdd gnd dc 1.8
Vin In gnd dc 1.2

.dc Vin 0 1.8 0.01

.control
run
set color0=white
plot V(In) V(S12) V(Outn) V(Outp) V(Dn5)
plot I(Vcm0) I(Vcm3) I(Vcm4)
.end
.endc
```

### Netlist Code Of Fully Differential Amplifier 
**AC Analysis**

```
*********************** Fully Dififerential Amplifier With Resistive Load **********************
******************************* AC ANALYSIS **************************
*********************** Date : 24/11/2025, Designer: Chandan Shaw  ****************************

.title Fully Differential Amplifier With Resistive Load

.lib /home/chandanvlsi/share/pdk/sky130A/libs.tech/ngspice/sky130.lib.spice tt

.global gnd gnd
.temp 27
xmn0 Dn0 Dn5 gnd gnd sky130_fd_pr__nfet_01v8 w=5 l=2 m=8
xmn6 Dn5 Dn5 gnd gnd sky130_fd_pr__nfet_01v8 w=5 l=2 m=8
xmn1 Outn Inp S12 gnd sky130_fd_pr__nfet_01v8 w=5 l=2 m=4
xmn2 Outp Inn S12 gnd sky130_fd_pr__nfet_01v8 w=5 l=2 m=4
Rld1 Sp3 Outn 8k
Rld2 Sp4 Outp 8k

Vcm0 S12 Dn0 dc 0
Vcm3 vdd Sp3 dc 0
Vcm4 vdd Sp4 dc 0
Icm5 vdd Dn5 dc 200u
Cl1 Outn gnd 20p
Cl2 Outp gnd 20p

Vsup vdd gnd dc 1.8

Vinp Inp gnd dc 1.5 ac 0.5
Vinn Inn gnd dc 1.5 ac -0.5

.ac dec 10 1 10G

.control
run
set color0=white
plot Vdb(Outp)
plot V(Inp) V(Inn) V(Outp) V(Outn)
.end
.endc
```

### Netlist Code Of Fully Differential Amplifier 
**TRANSIENT Analysis**

```
*********************** Fully Dififerential Amplifier With Resistive Load **********************
******************************* TRANSIENT ANALYSIS **************************
*********************** Date : 24/11/2025, Designer: Chandan Shaw  ****************************

.title Fully Differential Amplifier With Resistive Load
.lib /home/chandanvlsi/share/pdk/sky130A/libs.tech/ngspice/sky130.lib.spice tt
.global gnd gnd
.temp 27

xmn0 Dn0 Dn5 gnd gnd sky130_fd_pr__nfet_01v8 w=5 l=2 m=8
xmn6 Dn5 Dn5 gnd gnd sky130_fd_pr__nfet_01v8 w=5 l=2 m=8
xmn1 Outn Inp S12 gnd sky130_fd_pr__nfet_01v8 w=5 l=2 m=4
xmn2 Outp Inn S12 gnd sky130_fd_pr__nfet_01v8 w=5 l=2 m=4
Rld1 Sp3 Outn 8k
Rld2 Sp4 Outp 8k

Vcm0 S12 Dn0 dc 0
Vcm3 vdd Sp3 dc 0
Vcm4 vdd Sp4 dc 0
Icm5 vdd Dn5 dc 200u
Cl1 Outn gnd 20p
Cl2 Outp gnd 20p

Vsup vdd gnd dc 1.8

Vinp Inp gnd dc 1.5 ac 0.5 sin (1.5 10m 10k)
Vinn Inn gnd dc 1.5 ac -0.5 sin (1.5 -10m 10k)

.tran 1u 1m

.control
run
set color0=white
plot V(Outp) V(Outn)
plot V(Inp) V(Inn) V(Outp) V(Outn)
.end
.endc
```

## 11. Fully Differential Op-Amp

Type 1

```
*************************** NMOS Fully Differential Operational Amplifier *********************
******************************************* DC Analysis ***************************************
*********** Date: 05/01/2026, Designer: Chandan Shaw, Silicon University Bhubaneswar ***********

.title NMOS Fully Differential Operational Amplifier
.lib /home/chandanvlsi/share/pdk/sky130A/libs.tech/ngspice/sky130.lib.spice tt

.global gnd vdd
.temp 27

** NMOS and PMOS Transistors
xmn0 Dn0 Dn5 gnd gnd sky130_fd_pr__nfet_01v8 w=5 l=2 m=2
xmn5 Dn5 Dn5 gnd gnd sky130_fd_pr__nfet_01v8 w=5 l=2 m=2
xmn1 Outn In S12 gnd sky130_fd_pr__nfet_01v8 w=5 l=2 m=2
xmn2 Outp In S12 gnd sky130_fd_pr__nfet_01v8 w=5 l=2 m=2
xmp3 Outn Gp34 Sp3 vdd sky130_fd_pr__pfet_01v8_lvt w=5 l=2 m=5
xmp4 Outn Gp34 Sp4 vdd sky130_fd_pr__pfet_01v8_lvt w=5 l=2 m=5

** Feedback Resistors
Rgs3 Outn Gp34 100k
Rgs4 Outp Gp34 100k

** Voltage Source to measure Branch Currents
Vcm0 S12 Dn0 dc 0
Vcm3 vdd Sp3 dc 0
Vcm4 vdd Sp4 dc 0

** Current Source
Icm5 vdd Dn5 dc 50u

** Load Capacitance
Cln Outn gnd 10p
Clp Outp gnd 10p

** Supply Voltage
Vsup vdd gnd dc 1.8

** Differential Input
Vin In gnd dc 1.4 ac 1 sin(1.4 1m 10k)

** Simulation Command
.dc Vin 0 1.8 0.01

** Control Command
.control
run
set color0=white
plot V(In) V(S12) V(Outn) V(Outp) V(Dn5)
plot V(Gm34)
plot I(Vcm0) I(Vcm3) I(Vcm4)
.end
.endc
```

Type 2

```
**************************** NMOS Fully Differential Operational Amplifier *****************************
************************* DC Analysis *********************************
*************** Date: 07/01/2026 , Designer: Chandan Shaw , Silicon University ,Bbhubaneshwar ********************

.title NMOS Fully Differential Operational Amplifier

.lib /home/chandanvlsi/share/pdk/sky130A/libs.tech/ngspice/sky130.lib.spice tt

.global gnd vdd
.temp 27

** NMOS And PMOS Transistors
xmn0 Dn0 Dn5 gnd gnd sky130_fd_pr__nfet_01v8 w=5 l=2 m=2
xmn5 Dn5 Dn5 gnd gnd sky130_fd_pr__nfet_01v8 w=5 l=2 m=2
xmn1 Outn In S12 gnd sky130_fd_pr__nfet_01v8 w=5 l=2 m=2
xmn2 Outp In S12 gnd sky130_fd_pr__nfet_01v8 w=5 l=2 m=2
xmp3 Outn Gp34 Sp3 gnd sky130_fd_pr__pfet_01v8_lvt w=5 l=2 m=5
xmp4 Outp Gp34 Sp4 gnd sky130_fd_pr__pfet_01v8_lvt w=5 l=2 m=5

** Feedback Resistors
Rn Outn Ravg 10k
Rp Outp Ravg 10k

** Common Mode Feedback Amplifier
xmn6 Dn6 Dn5 gnd gnd sky130_fd_pr__nfet_01v8 w=5 l=2 m=2
xmn7 Dn9 Ravg S78 gnd sky130_fd_pr__nfet_01v8 w=5 l=2 m=2
xmn8 Gp34 Ref S78 gnd sky130_fd_pr__nfet_01v8 w=5 l=2 m=2
xmp9 Dn9 Dn9 Sp9 vdd sky130_fd_pr__pfet_01v8_lvt w=5 l=2 m=5
xmp10 Gp34 Dn9 Sp10 vdd sky130_fd_pr__pfet_01v8_lvt w=5 l=2 m=5

** Voltage Source to measure branch Currents
Vcm0 S12 Dn0 dc 0
Vcm3 vdd Sp3 dc 0
Vcm4 vdd Sp4 dc 0
Vcm6 S78 Dn6 dc 0
Vcm9 vdd Sp9 dc 0
Vcm10 vdd Sp10 dc 0

** Current Source(Mirror Current)
Icm5 vdd Dn5 dc 50u

** Load Capacitance
Clp Outp gnd 10p
Cln Outn gnd 10p

** Supply Voltage
Vsup vdd gnd dc 1.8

** Reference Voltage
Vref Ref gnd dc 1.8

** Differential Input
Vin In gnd dc 1.4
*Vinn Inn gnd dc 1.4

** Simulation Command
.dc Vin 0 1.8 0.01

** Control Statement
.control
run
set color0=white
plot V(In) V(S12) V(S78) V(Outn) V(Outp) V(Dn5)
plot V(Dp9) V(Gp34) V(Ravg)
plot I(Vcm0) I(Vcm3) I(Vcm4) I(Vcm6) I(Vcm9) I(Vcm10)
.end
.endc
```

## 12. Two stage Amplifier
- A two-stage op-amp is one of the most common CMOS amplifier topologies used in analog IC design, especially when you need high gain and reasonable output swing.
- It’s called “two-stage” because the signal passes through two cascaded gain stages before reaching the output.
### Structure
- A typical two-stage op-amp consists of:
### I.Stage 1 – Differential Amplifier with Active Load
- Purpose: Provides high input impedance, large differential gain, and converts differential input to a single-ended output.
- Often implemented with NMOS differential pair and PMOS current-mirror load.
- This stage sets much of the DC gain.
### II.Stage 2 – Common-Source Gain Stage
- Purpose: Further amplifies the single-ended signal from Stage 1 to the final output level.
- Usually an NMOS common-source transistor with a PMOS current-source load.
- This stage provides extra gain and helps drive moderate loads.
### III.Compensation Network
- Usually a Miller compensation capacitor between Stage-1 output and Stage-2 output.
- Sometimes includes a series resistor to move the zero to the left half-plane for stability.
- Ensures adequate phase margin for closed-loop stability.
### Advantages
- High DC Gain
- High Input Impedance
- Moderate Output Swing
### Disadvantages
- Larger Area
- Stability Issues
- Slew Rate Limitation
- More Power Consumption
- Lower Speed than Single-Stage
### Summary Table
| Feature          | Two-Stage Op-Amp                   |
| ---------------- | ---------------------------------- |
| **Gain**         | Very high (60–90 dB)               |
| **Bandwidth**    | Moderate (set by Miller C)         |
| **Load Drive**   | Moderate (pF range)                |
| **Complexity**   | Higher than single-stage           |
| **Stability**    | Requires compensation              |

#### Stability Analysis Without Compensate Capacitor
```
*************************** NMOS Two-Stage Operational Amplifier (With Compensative capacitor) ***************************
******************************************* Stability and Settling Time ANALYSIS *****************************************
******************************* Date: 23/12/2025 ,Designer: Chandan Shaw ,Silicon University *****************************

.title NMOS Two-Stage Operationational Amplifier

.lib /home/chandanvlsi/share/pdk/sky130A/libs.tech/ngspice/sky130.lib.spice tt

.global gnd vdd
.temp 27

** NMOS and PMOS Transistors
xmn0 Dn0 Dn5 gnd gnd sky130_fd_pr__nfet_01v8 w=5 l=2 m=2
xmn5 Dn5 Dn5 gnd gnd sky130_fd_pr__nfet_01v8 w=5 l=2 m=2
xmn1 Dp3 Out S12 gnd sky130_fd_pr__nfet_01v8 w=5 l=2 m=2
xmn2 Out1 Inp S12 gnd sky130_fd_pr__nfet_01v8 w=5 l=2 m=2
xmp3 Dp3 Dp3 Sp3 vdd sky130_fd_pr__pfet_01v8_lvt w=5 l=2 m=5
xmp4 Out1 Dp3 Sp4 vdd sky130_fd_pr__pfet_01v8_lvt w=5 l=2 m=5
xmp6 Out out1 Sp6 vdd sky130_fd_pr__pfet_01v8_lvt w=5 l=2 m=80
xmp7 Out Dn5 gnd gnd sky130_fd_pr__nfet_01v8 w=5 l=2 m=16

** Voltage Source to measure Branch Currents
Vcm0 S12 Dn0 dc 0
Vcm3 vdd Sp3 dc 0
Vcm4 vdd Sp4 dc 0
Vcm6 vdd Sp6 dc 0

** Current Source
Icm5 vdd Dn5 dc 50u

** Load capacitance
Cl Out gnd 10p
Cc Out1 Out 5p

** Supply Voltage
Vsup vdd gnd dc 1.8

** Differential Input
Vinp Inp gnd pulse (0 1.2 10n 100p 100p 0.5 1)
*Vinn Inn gnd dc 1.5 c -0.5 sin(1.5 1m 10k)

** Simultion Command take 0.5u for better and also 5u
.tran 10p 0.5u

** Control Statement
.control
run
setcolor0=white
plot V(Inp) V(Out)
.end
.endc
```

#### Stability Analysis Without Compensate Capacitor
```
*********************** NMOS Two-Stage Operational Amplifier (Without Compensative Capacitor) ****************************
******************************************* Stability and Settling Time ANALYSIS *****************************************
******************************* Date: 23/12/2025 ,Designer: Chandan Shaw ,Silicon University *****************************

.title NMOS Two-Stage Operationational Amplifier

.lib /home/chandanvlsi/share/pdk/sky130A/libs.tech/ngspice/sky130.lib.spice tt

.global gnd vdd
.temp 27

** NMOS and PMOS Transistors
xmn0 Dn0 Dn5 gnd gnd sky130_fd_pr__nfet_01v8 w=5 l=2 m=2
xmn5 Dn5 Dn5 gnd gnd sky130_fd_pr__nfet_01v8 w=5 l=2 m=2
xmn1 Dp3 Out S12 gnd sky130_fd_pr__nfet_01v8 w=5 l=2 m=2
xmn2 Out1 Inp S12 gnd sky130_fd_pr__nfet_01v8 w=5 l=2 m=2
xmp3 Dp3 Dp3 Sp3 vdd sky130_fd_pr__pfet_01v8_lvt w=5 l=2 m=5
xmp4 Out1 Dp3 Sp4 vdd sky130_fd_pr__pfet_01v8_lvt w=5 l=2 m=5
xmp6 Out out1 Sp6 vdd sky130_fd_pr__pfet_01v8_lvt w=5 l=2 m=80
xmp7 Out Dn5 gnd gnd sky130_fd_pr__nfet_01v8 w=5 l=2 m=16

** Voltage Source to measure Branch Currents
Vcm0 S12 Dn0 dc 0
Vcm3 vdd Sp3 dc 0
Vcm4 vdd Sp4 dc 0
Vcm6 vdd Sp6 dc 0

** Current Source
Icm5 vdd Dn5 dc 50u

** Load capacitance
Cl Out gnd 10p
*Cc Out1 Out 5p

** Supply Voltage
Vsup vdd gnd dc 1.8

** Differential Input
Vinp Inp gnd pulse (0 1.2 10n 100p 100p 0.5 1)
*Vinn Inn gnd dc 1.5 c -0.5 sin(1.5 1m 10k)

** Simultion Command
.tran 10p 0.5u

** Control Statement
.control
run
setcolor0=white
plot V(Inp) V(Out)
.end
.endc
```

# 13. Balanced Amplifier
- A balanced amplifier is an amplifier configuration designed so that its output (or input) is symmetrical with respect to a reference point.
### Why Balanced?
- The signals are balanced in amplitude and anti-phase (180° apart).
- This symmetry provides better common-mode noise rejection — any interference picked up equally on both lines is canceled out when taking the difference.
### Advantages
- High CMRR
- Lower distortion
### Disadvantages
- More complex design
- Consumes more power than a single-ended stage.

## 13.1 Balanced Amplifier using NMOS

**DC Analysis**

```
***************************** Balanced Amplifier ******************************************
***************************************** DC ANALYSIS *************************************
************* Date: 27/12/2025 , Designer: Chandan Shaw , Silicon University **************

.title Balanced Amplifier
.lib /home/chandanvlsi/share/pdk/sky130A/libs.tech/ngspice/sky130.lib.spice tt
.global vdd gnd
.temp 27

** For NMOS
xmn0 Dn1 Dn9 gnd gnd sky130_fd_pr__nfet_01v8 w=5 l=2 m=2
xmn1 Dn2 In Dn1 gnd sky130_fd_pr__nfet_01v8 w=5 l=2 m=8
xmn2 Dn3 In Dn1 gnd sky130_fd_pr__nfet_01v8 w=5 l=2 m=8
xmn7 Dn7 Dn7 gnd gnd sky130_fd_pr__nfet_01v8 w=5 l=2 m=16
xmn8 Vout Dn7 gnd gnd sky130_fd_pr__nfet_01v8 w=5 l=2 m=16
xmn9 Dn9 Dn9 gnd gnd sky130_fd_pr__nfet_01v8 w=5 l=2 m=2

** For PMOS
xmp3 Dn2 Dn2 Sp3 vdd sky130_fd_pr__pfet_01v8_lvt w=5 l=2 m=5
xmp4 Dn3 Dn3 vdd vdd sky130_fd_pr__pfet_01v8_lvt w=5 l=2 m=5
xmp5 Dn7 Dn2 Sp4 vdd sky130_fd_pr__pfet_01v8_lvt w=5 l=2 m=80
xmp6 vout Dn3 Sp5 vdd sky130_fd_pr__pfet_01v8_lvt w=5 l=2 m=80

** Load Capacitance
Cl vout gnd 10p

** Current Supply
Isup vdd Dn9 dc 50u


** Voltage Source
Vcm0 vdd Sp3 dc 0
Vcm1 vdd Sp4 dc 0
Vcm2 vdd Sp5 dc 0

** Voltage Supply
Vsup vdd gnd dc 1.8

Vin In gnd dc 1.2

** Simulation Command
.dc Vin 0 1.8 0.01

** Control Command
.control
run

plot V(In) V(Dn7) V(Dn3) V(Dn2) V(Dn1)
plot I(Vcm0) I(Vcm1) I(Vcm2)
.end
.endc
```

![Diagram](https://github.com/Chandan-Shaw/Characterization/blob/main/Balanced_Amp_dc_Simulation.png)
![Diagram](https://github.com/Chandan-Shaw/Characterization/blob/main/Balanced_Ampl_dc_Simulation.png)

**AC Analysis**

```
***************************** Balanced Amplifier ******************************************
************************************** AC ANALYSIS ****************************************
************* Date: 27/12/2025 , Designer: Chandan Shaw , Silicon University **************

.title Balanced Amplifier
.lib /home/chandanvlsi/share/pdk/sky130A/libs.tech/ngspice/sky130.lib.spice tt
.global vdd gnd
.temp 27

** For NMOS
xmn0 Dn1 Dn9 gnd gnd sky130_fd_pr__nfet_01v8 w=5 l=2 m=2
xmn1 Dn2 In Dn1 gnd sky130_fd_pr__nfet_01v8 w=5 l=2 m=8
xmn2 Dn3 In Dn1 gnd sky130_fd_pr__nfet_01v8 w=5 l=2 m=8
xmn7 Dn7 Dn7 gnd gnd sky130_fd_pr__nfet_01v8 w=5 l=2 m=16
xmn8 Vout Dn7 gnd gnd sky130_fd_pr__nfet_01v8 w=5 l=2 m=16
xmn9 Dn9 Dn9 gnd gnd sky130_fd_pr__nfet_01v8 w=5 l=2 m=2

** For PMOS
xmp3 Dn2 Dn2 Sp3 vdd sky130_fd_pr__pfet_01v8_lvt w=5 l=2 m=5
xmp4 Dn3 Dn3 vdd vdd sky130_fd_pr__pfet_01v8_lvt w=5 l=2 m=5
xmp5 Dn7 Dn2 Sp4 vdd sky130_fd_pr__pfet_01v8_lvt w=5 l=2 m=80
xmp6 vout Dn3 Sp5 vdd sky130_fd_pr__pfet_01v8_lvt w=5 l=2 m=80

** Load Capacitance
Cl vout gnd 10p

** Current Supply
Isup vdd Dn9 dc 50u


** Voltage Source
Vcm0 vdd Sp3 dc 0
Vcm1 vdd Sp4 dc 0
Vcm2 vdd Sp5 dc 0

** Voltage Supply
Vsup vdd gnd dc 1.8

Vin In gnd dc 1.4 ac 0.5 sin(1.4 1m 10k)

** Simulation Command
.ac dec 10 1 10Meg

** Control Command
.control
run

plot db(vout)
plot 180*cph(vout)/pi
.end
.endc
```
![Diagram](https://github.com/Chandan-Shaw/Characterization/blob/main/Balanced_Amp_ac_Simulation.png)
![Diagram](https://github.com/Chandan-Shaw/Characterization/blob/main/Balanced_Ampl_ac_Simulation.png)
