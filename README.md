# COE538 eBot Line Follower (HCS12, Assembly, FSM)

## Overview
This repository contains the firmware for an **HCS12-based eBot** that autonomously follows a black line, responds to bumper collisions, and recovers using a finite state machine (FSM).

The system is written entirely in **HCS12 assembly** and runs directly on hardware with no operating system or abstraction layers.

A short demo video of the robot in operation is available on my LinkedIn.

## What the system does
- Follows a black line using **5 optical guider sensors**
- Detects and handles **front/rear bumper collisions**
- Performs controlled turns and re-alignment
- Displays **live state, sensor values, and battery voltage** on an LCD
- Runs continuously in real time

## How it works
### FSM-based control
The robot behavior is governed by an explicit finite state machine with states including:
- START, FWD, ALL_STOP  
- LEFT_TRN, RIGHT_TRN, REV_TRN  
- LEFT_ALIGN, RIGHT_ALIGN  

State transitions are driven by sensor readings, bumper inputs, and timing conditions.

### Sensor processing (ADC)
Each guider sensor is sampled using the HCS12 ADC.  
Baseline and variance threshold values were **experimentally tuned** to account for sensor noise, lighting changes, and hardware variation.

### Timing and interrupts
A **Timer Overflow interrupt** is used as a global time reference for motion timing and controlled turns.

### Motor control
Motor direction and enable signals are driven directly through **PORTA and PTT**, with dedicated routines for forward, reverse, turning, and stopping behavior.

### LCD feedback
The LCD displays:
- Current FSM state
- Live sensor readings
- Battery voltage  

This feedback was also used extensively during debugging.

## Debugging and tuning
During testing, the robot would occasionally stop responding or behave inconsistently.  
This was resolved by stepping through the assembly code, refining **interrupt handling and timer logic**, and adjusting FSM transitions.

Additional tuning was required for:
- Black/white sensor threshold values
- Turn aggressiveness and timing
- Alignment behavior after collisions

## Repository structure
src/
ebot.asm # Main HCS12 assembly firmware

## Notes
- Threshold values and timing constants were tuned empirically on real hardware.
- This repository contains **code only**; design decisions and behavior are summarized here.

## Demo
Demo video available on LinkedIn:
https://www.linkedin.com/posts/anoosh-zaidi_embeddedsystems-microcontrollers-hcs12-activity-7407535966627872768-OoVH?utm_source=share&utm_medium=member_desktop&rcm=ACoAAEA9uoABcTK0-TmB7S85hs4lwIK8zz9O5yQ
