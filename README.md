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
## Content
 ## 1. Basic Concept

 ### 1.1 Charge
  The most basic quantity in an electric circuit is the electric charge.
  
  Charge is an **Electric Property** of an atomic particle of which matter consists, measured in **Coulombs** (C).
  
  The Coulomb is a large unit of charge. ``1C = 1 / 1.602 * 10^-19 =  6.24 * 10^-18``  

 ### 1.2 Current
  The electric current due to the flow of electronic charge in a conductor.

  Electric Current is the time rate of change of Charge,measured in **Ampere** (A).

  Mathematically - `` i = dq/dt ``
 
  ( 1 Ampere = 1 Coulomb/1Second )

 ### 1.3 Voltage
  Voltage is the energy required to move a unit charge from a reference point(-) to another point (+), measured in
  **Volts (V).**

  Voltage between two pont a & b is as:  `` Vab = dw/dq ``  ( 1 Volt = 1 Joule/Coulomb )

  **Vab = -Vba** (Voltage drops from a to b is equivalent to voltage rise from b to a )

 ### 1.4 Power
  Power is the time rate of expanding or absorbing energy, measured is **Watts (W).**

  Mathematically , `` p = dw/dt `` (1 Watt = 1 Joule/1Second)

  where p is **power**, w is **energy** in Joules and t is the **time** in seconds.

  `` p = vi `` , is a time varing quantity and is called **Instantaneous Power.**

  The power absorbed or supplied by an element is the product of the voltage across the element and the current          through it.

 ### 1.5 Energy
  Energy is the capacity to do work, measured in **Joules (J).**

  Energy is an electric circuit is defined as the total power consumption in a period (From time t0 to t)

  Mathematically ,  w = $\int_{t_0}^{t} p \ dt$ = $\int_{t_0}^{t} vi \ dt$  

  The electric power utility companies measures energy in Watt-hour (Wh) , Where `` 1 Wh = 3600 J `` 
 ## 2. Linear Element
  
 ### 2.1 Resistor
  A resistor is an passive electrical component that opposes the flow of electric current.

  The resistance value is measured in **Ohms (Ω)**

  Given by Ohm’s Law:   `` V=I×R `` 
  
  **Used of Resistor**
   - Its main use is to limit or control the flow of electric current in a circuit.
   - To set biasing in amplifiers

 ### 2.2 Capacitors
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

  ### 2.3 RC Circuit
  - An RC circuit is an electrical circuit made of **R — Resistor** & **C — Capacitor** connected either in series or parallel, which exhibit a time-dependent response to voltage or current changes. The     fundamental time constant is defined as:  
    `τ = R * C`, where `τ` (tau) represents the **time constant** in seconds, indicating how quickly the circuit charges or discharges.
  - In the **Skywater SKY130 PDK**, **RC circuits** are implemented using integrated resistors (e.g.,`sky130_fd_pr__res_high_po`) and capacitors (e.g.,`sky130_fd_pr__cap_mim_m3_1`). These are critical in    analog and mixed-signal design applications such as filters, timing circuits, and analog front ends.

  ![Diagram](https://github.com/Chandan-Shaw/Characterization/blob/main/RC%20Circuit.JPG)
 
  #### 2.3.1 Transient Analysis
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
    
  #### 2.3.2 AC Analysis
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

  ### 2.4 CR Circuit
  - A **Cr circuit** is essentially the same as an RC circuit, but with the capacitor (C) placed before the resistor (R) in the signal path. While electrically the time constant remains the same, the        circuit response differs, especially in transient analysis. The fundamental time constant is defined as: `τ = R * C`,
  where `τ` (tau) represents the **time constant** in seconds, indicating how quickly the circuit charges or discharges.

  - In the **Skywater SKY130 PDK**, **CR circuits** are implemented using integrated capacitors (e.g., `sky130_fd_pr__cap_mim_m3_1`) and resistors (e.g., `sky130_fd_pr__res_high_po`). These configurations   are often used in differentiator circuits, pulse shaping, and AC coupling applications in analog and RF systems.

  ![Diagram](https://github.com/Chandan-Shaw/Characterization/blob/main/CR%20Circuit.JPG)

  #### 2.4.1 Transient Analysis
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

  #### 2.4.2 AC Analysis

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

 ## 3. MOSFET Circuits
 - A MOSFET(Metal-Oxide-Semiconductor Field-Effect Transistor) has four terminal device:- Gate, Drain, Source, Body.
 - MOSFET is device used for switching and amplification, Its current is controlled by the voltage applied to the gate terminal.
 - The MOSFET operates in three regions: **cutoff, linear, and saturation**, depending on gate-source (VGS) and drain-source (VDS) voltages.
 - In the Skywater SKY130 PDK, MOSFETs like ``sky130_fd_pr__nfet_01v8`` (NMOS) and ``sky130_fd_pr__pfet_01v8`` (PMOS) are commonly used. These are essential in digital logic, analog amplifiers, and
 switching applications.
 
 
   
  ## 1. CMOS
  
  ### What Is CMOS ?
  CMOS (Complementary Metal–Oxide–Semiconductor) is a technology used to build most modern digital chips — like
  processors, memory, and logic circuits.

  CMOS is a semiconductor technology that uses complementary NMOS and PMOS transistors to build low-power, high-speed
  digital and analog circuits.
  
  It Uses two types OF MOSFET Operation:
  - **PMOS(Positive Type)**
  - **NMOS(Negative Type)**
    
  They work together in a CMOS: - When one is **ON** then Other is **OFF.**

  ### Why CMOS is important ?
  - Very Low Power Consumption
  - High Speed
  - High Packing Density ( Many Circuits on a small Chip. )
  - Reliability

    It is used in like ***Microprocessors***,***Smartphone***,***Digital Logic Circuits***,***Memory Chips.***


  ## Simple Current Mirror

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

 ## Wide Swing Cascode Current Mirror

 ### DC Analysis
 ```
 ********************************* Wide Swing Cascode Current Mirror Using NMOS *********************************
 ************************ DC ANALYSIS ***************************
 ************* Date: 11/01/2026 , Designer: Chandan Shaw , Silicon University Bhubaneshwar ********************

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
 
