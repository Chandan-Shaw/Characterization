# IC-Characterization
- This GitHub repository is created to provide hands-on experience in Analog IC Characterization using open-source tools such as **NGSpice**, **Xschem**, and the **SKY130 PDK**.
--------------------------------------------------------------------------------------------------------
## What Is Analog Characterization ?

- Analog IC (Integrated Circuit) Characterization is the process of evaluating and measuring the electrical performance of an analog circuit under various conditions. This involves testing key parameters such as gain, offset, bandwidth, noise, power consumption, input/output impedance, temperature stability, and process variation sensitivity.
- 
--------------------------------------------------------------------------------------------------------
## Content
- ## 1. Basic Concept & Linear Element

  ### 1.1 Charge
  The most basic quantity in an electric circuit is the electric charge.
  
  Charge is an **Electric Property** of an atomic particle of which matter consists, measured in **Coulombs** (C).
  
  The Coulomb is a large unit of charge. 1C = 1 / 1.602 * 10^-19 =  6.24 * 10^-18  

  ### 1.2 Current
  The electric current due to the flow of electronic charge in a conductor.

  Electric Current is the time rate of change of Charge,measured in **Ampere** (A).

  Mathematically - i = dq/dt

  ( 1 Ampere = 1 Coulomb/1Second )

  ### 1.3 Voltage
  Voltage is the energy required to move a unit charge from a reference point(-) to another point (+), measured in
  **Volts (V).**

  Voltage between two pont a & b is as:  Vab = dw/dq   ( 1 Volt = 1 Joule/Coulomb )

  **Vab = -Vba** (Voltage drops from a to b is equivalent to voltage rise from b to a )
  
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
