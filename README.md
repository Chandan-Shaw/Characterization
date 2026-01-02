# IC-Characterization
- This GitHub repository is created to provide hands-on experience in Analog IC Characterization using open-source tools such as **NGSpice**, **Xschem**, and the **SKY130 PDK**.
--------------------------------------------------------------------------------------------------------
## What Is Analog Characterization ?

- Analog IC (Integrated Circuit) Characterization is the process of evaluating and measuring the electrical performance of an analog circuit under various conditions. This involves testing key parameters such as gain, offset, bandwidth, noise, power consumption, input/output impedance, temperature stability, and process variation sensitivity.

--------------------------------------------------------------------------------------------------------
## Content
- ## 1. Basic Concept & Linear Element

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

  Mathematically , `` w = $\int_{t_0}^{t} p \ dt$ = $\int_{t_0}^{t} vi \ dt$ `` 

  The electric power utility companies measures energy in Watt-hour (Wh) , Where `` 1 Wh = 3600 J `` 

  ### 1.6 Resistor
  A resistor is an passive electrical component that opposes the flow of electric current.

  The resistance value is measured in **Ohms (Ω)**

  Given by Ohm’s Law:   `` V=I×R `` 
  
  **Used of Resistor**
   - Its main use is to limit or control the flow of electric current in a circuit.
   - To set biasing in amplifiers

   ### 1.7 Capacitors
   A capacitor is a passive electrical component that stores energy in the form of an electric field, defined by the      relation: `` Q = C * V ``, where C is the capacitance in **Farads**.

   The capacitance C of a parallel-plate capacitor depends on its physical structure and the material between the         plates, given by the formula: `` C = εA / d ``.

   In the **Skywater SKY130 PDK**, various capacitor types are available for use in analog, RF, and digital designs,     each offering trade-offs in capacitance density, linearity, voltage rating, and temperature stability.

   ### Types of Capacitors available:
   - ``sky130_fd_pr__cap_mim_m3_1.model`` is a **Metal-Insulator-Metal (MIM)** capacitor between **Metal3 and            Metal2**, suitable for analog precision applications.
   - ``sky130_fd_pr__cap_mim_m3_2.model`` is another **MIM** capacitor variant with different area usage and             parasitic trade-offs.
   - ``sky130_fd_pr__cap_mim_m2_1.model`` defines a MIM capacitor between **Metal2 and Metal1** layers.
   - ``sky130_fd_pr__cap_var_lvt.model`` is a **MOS varactor** (voltage-dependent capacitor) built using LVT NMOS        structure, useful for RF tuning.
   - ``sky130_fd_pr__cap_var_hvt.model`` is a similar **varactor** using HVT device for different threshold and          leakage behavior.
   ![Diagram](https://github.com/Chandan-Shaw/Characterization/blob/main/Capacitor.JPG)
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

   ### Plot Of Vin And Vout
  
  ![Diagram](https://github.com/Chandan-Shaw/Characterization/blob/main/RC_Simulation.png)

   
- ## 1. CMOS
  
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
