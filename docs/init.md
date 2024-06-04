---
hide:
  - toc
---

# Initialization and Usage

## Start-up and Shutdown Procedures

In the main folder of the pyFCM suite one can edit the files `startup.txt` and `shutdown.txt` to reflect start-up and 
shutdown procedures that the user wants the user to follow (add one instruction per line in the txt files). 
These instructions will be prompted if the variable `startup_instructions` is set to `True` (which is the default).

## Initialization of Software Components

The main class `PCM` in the `process_control_module` is the central hub and every hardware component 
has its own Python class that needs to be initialized and then passed to the `PCM` class as a variable for initialization.

To illustrate this, below we will give a few examples:

- **Electrolyzer with `electrical_control_module` provided**
```Python3
from pyFCM.process_control_module import PCM
from pyFCM.electrical_control_module import LCM
from pyFCM.data_acquisition_hardware import LabJackT7

# Software initialization
lj = LabJackT7()
lcm = LCMTUM(lj)

pcm = PCM(electrical_module=lcm,
          sample_id="test"
          )

# Software initialization complete, runs starts below:
pcm.wait(duration=5)
pcm.set_gas_pressure_anode(gas_pressure=0.5)
pcm.set_gas_pressure_cathode(gas_pressure=0.5)
pcm.wait(duration=5)
pcm.hardware_reset()
# End of test run
```

-  **Fuel Cell with `electrical_control_module`, `load_hardware`, `anode_flow_module`, and `cathode_flow_module` provided**
```Python3
from pyFCM.process_control_module import PCM
from pyFCM.electrical_control_module import ECM
from pyFCM.data_acquisition_hardware import LabJackT7
from pyFCM.anode_flow_module import AFM
from pyFCM.cathode_flow_module import CFM
from pyFCM.load_hardware import Load

# Software initialization
lj = LabJackT7()
ecm = ECMTUM(lj)
afm = AFM()
cfm = CFM()
lh = Load()

pcm = PCM(heater_module=None,
          anode_module=afm,
          cathode_module=cfm,
          electrical_module=ecm,
          load_module=lh,
          potentiostat_module=None,
          sample_id="test123"
          )

# Software initialization complete, runs starts below:
pcm.wait(duration=5)
pcm.switch_to_Ar_anode()
pcm.switch_to_fuel_anode()
pcm.set_gas_flowrate_anode(flow=0.5)
pcm.set_gas_flowrate_cathode(flow=0.5)
pcm.wait(duration=5)
pcm.FC_mode_CC(duration=10, load_current=[0.1, 0.2, 0.5])
pcm.wait(duration=5)
pcm.hardware_reset()
# End of test run
```

A full list of available function (depending on which hardware is available at the user) can be found [here](functions.md)