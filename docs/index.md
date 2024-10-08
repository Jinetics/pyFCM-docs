---
hide:
  - toc
---

# Documentation for pyFCM Software Suite

This website explains how the pyFCM software suite can be set up and used.

## Installation

The software which is based on Python 3.10 is already pre-installed on the supplied computers. 
It can be used through Python's import statement. 
For example, the main class can be imported by `from pyFCM.process_control_module import PCM`. 
Details and examples on initializing the class are explained [here](init.md).

## Hardware Components and Their Interaction with pyFCM

The software can be used with different hardware components. There are two options on how the software interfaces 
with each hardware component:

1. The hardware component is supplied by Jinetics and the related software interface code is therefore also 
supplied by Jinetics.
2. The hardware component is sourced by the user. In this case, the interface code needs to be completed by 
the user through template interface files (more details on [this page](custom.md)).

A list of high-level functions is available in the pyFCM suite and some of them rely on the availability of 
certain hardware components. A complete list of available functions is available [here](functions.md).

## Software Structure

The main entry point for using the pyFCM software is the **process control module** and its `PCM` class which has 
high-level functions related to electrolyzer and/or fuel cell testing protocols. It also logs data from all 
hardware components which are initialized through the software suite. 
Further, it monitors certain signals that can act as kill switch conditions to shut off the system.

Here is a full list of all software interfaces with the hardware components:

- `electrical_control_module` (always provided by Jinetics)
    - For fuel cell testing: This interface works with the 
main electrical unit to measure voltages/currents during fuel cell testing and set the states of 
3 relays (one of which acts as an MFC shutoff). It also reads the backpressures from the backpressure 
hardware (if provided by Jinetics).
    - For fuel cell testing: This interface works with the 
main electrical unit to measure voltages/currents during electrolyzer testing and read & set the 
anode and cathode gas pressures. It sets the states of relays (one of which acts as an MFC shutoff).
    - `data_acquisition_hardware`: This module interfaces with the data acquisition hardware provided by Jinetics. 
This class is initialized and used as an input variable during initialization of 
the `electrical_control_module`.
- `heater_control_module` (optionally provided by Jinetics): This interface is designed to measure and set up
to 8 temperatures. If not provided by Jinetics, then any 8 values can be set/measured (not necessarily related to 
temperature).
- `cathode_flow_module` and `anode_flow_module` (for fuel cell testing; optionally provided by Jinetics): These modules
set and read the flow rates of gases and water for the anode and cathode side during fuel cell testing. It is equipped 
to switch between multiple gases and uses our in-house algorithm module to set the RH accurately.
- `backpressure_module` (for fuel cell testing; optionally provided by Jinetics): This module sets the backpressures 
of the anode and cathode during fuel cell testing.
- `load_hardware` (for fuel cell and electrolyzer testing; optionally provided by Jinetics): This module manages the 
load during fuel cell testing or manages the power source during electrolyzer testing. It sets voltage and currents 
during testing.
- `potentiostat_hardware` (for fuel cell and electrolyzer testing; optionally provided by Jinetics): This module manages
the potentiostat during fuel cell and electrolyzer testing. It enables functions like cyclic voltammetry, 
squarewave mode, among others.

Furthermore, Jinetics supports additional software components (at additional charge):

- `recipe_loading_module`: Enables the loading of test recipe files for ease of use and editing.
- `data_plotting_module`: Live-view during data acquisition.
- `data_analysis`: Analysis code for fuel cell testing protocols and detailed analysis of acquired data results.
- A graphic user interface (GUI) that enables the operation of this software without a single line of code.

## Software Safety Feature

In addition to hardware components with their own safety features, the pyFCM software runs a `Killswitch` process that can prematurely 
terminate the test protocol and run the function `hardware_reset` which resets all gases and temperatures back to 0. 
For hardware supplied by Jinetics the corresponding software modules have the following safety checks implemented:

- `electrical_control_module`: The user can attach any device (e.g., a hydrogen detector) to the Jinetic's supplied electrical control unit. 
Upon reading a high state (5-10 V) from the device, the `Killswitch` shutoff will be triggered.
- `heater_control_module`: If temperature 1 (cell temperature) reads more than 100 C, the `Killswitch` shutoff will be triggered.
- `cathode_flow_module` and `anode_flow_module`: If the measured gas flow deviates from the setpoint for an extended period of time, 
the `Killswitch` shutoff thread will be triggered.

!!! note "For Custom Hardware Templates"
    
    For modules where software templates are used (i.e., where Jinetics has not provided the hardware component), users have full flexibility 
    to define their own safety conditions. These conditions should be specified within the class's `reading` function so that when the user's 
    criteria are satisfied, `self.ks` is set to `True`.

