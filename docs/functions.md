# List of all Available Functions in the pyFCM Software Suite

Here is a comprehensive list of all available functions and the input variables for each. Also, it is states 
which hardware components need to be initialized for each function.

!!! note
    
    The `self` keyword is related to object-oriented programming and is not 
    to be filled by the user when used.

## Initialization Options of the `PCM` Class

```Python3
def __init__(self,
             heater_module: type[HeaterProtocol] | HeaterProtocol | None = None,
             anode_module: type[AnodeProtocol] | AnodeProtocol | None = None,
             cathode_module: type[CathodeProtocol] | CathodeProtocol | None = None,
             power_device: type[PowerProtocol] | PowerProtocol | None = None,
             electrical_module: type[ElectricalProtocol] | ElectricalProtocol | None = None,
             load_module: type[LoadProtocol] | LoadProtocol | None = None,
             potentiostat_module: type[PotentiostatProtocol] | PotentiostatProtocol | None = None,
             backpressure_module: type[BackpressureProtocol] | BackpressureProtocol | None = None,
             run_basehall: bool = False, start_killswitch_thread: bool = True,
             file_output: bool = True, startup_instructions: bool = True, verbose: bool = True,
             outdir: str | None = None, recipe_id: str | None = None, sample_id: str | None = None,
             log_refresh_rate: float = 0.3
             ):
    """
    Initialization of the `PCM` class.
    :param heater_module: Instance of `heater_control_module` class
        Default: None
    :param anode_module: Instance of `anode_flow_module` class
        Default: None
    :param cathode_module: Instance of `cathode_flow_module` class
        Default: None
    :param power_device: Instance of `power_device` class
        Default: None
    :param electrical_module: Instance of `electrical_control_module` class
        Default: None
    :param load_module: Instance of `load_hardware` class
        Default: None
    :param potentiostat_module: Instance of `potentiostat_hardware` class
        Default: None
    :param backpressure_module: Instance of `backpressure_module` class
        Default: None
    :param run_basehall: (bool) Whether to run basehall measurement in the beginning
        Default: False
    :param start_killswitch_thread: (bool) Whether to start the killswitch thread in the beginning
        Default: True
    :param file_output: (bool) Whether to generate log files
        Default: True
    :param startup_instructions: (bool) Whether to list the startup instructions
        Default: True
    :param verbose: (bool) Whether to print out additional text
        Default: True
    :param outdir: (str | None) Path to output directory
        Default: None
    :param recipe_id: (str | None) Recipe file name/ID
        Default: None
    :param sample_id: (str | None) Sample file name/ID
        Default: None
    :param log_refresh_rate: (float) Log refresh rate in seconds
        Default: 0.3
    """
```


## Basic Functions only Requiring `electrical_control_module`

```Python3
def start_log(self) -> None:
    """
    This starts the logging of data in the background
    (automatically initiated if `file_output` of the PCM class is set to True [which is the default])
    :return: None
    """
```

```Python3
def stop_log(self) -> None:
    """
    This stops/pauses the logging of data in the background
    :return: None
    """
```

```Python3
def wait(self, duration: float, skipable: bool = False, skip_voltage: float | None = None) -> None:
    """
    Requires `electrical_control_module` if `skip_voltage` is specified
    Waits for the specified duration. If `skipable` is True then this wait can be skipped by pressing
    the `end` key on the keyboard.
    If `skip_voltage` is specified the wait is skipped if the response voltage is less than that value.
    :param duration: (float) Duration in seconds
    :param skipable: (bool) Whether to be able to skip the wait (by pressing `end` key on the keyboard)
        Default: False
    :param skip_voltage: (float | None) Voltage that determines the skipping trigger
        Default: None
    :return: None
    """
```

```Python3
def log_text(self, text: str) -> None:
    """
    Add text to the log file.
    :param text: (str) The text to be logged.
    :return: None
    """
```

```Python3
def hardware_reset(self, default_gas_anode: str = 'H2', default_gas_cathode: str = 'air') -> None:
    """
    Resets all hardware components that were initialized.
    :param default_gas_anode: (str) Default gas for anode (only relevant if `anode_flow_module` is utilized)
        Default: 'H2'
    :param default_gas_cathode: (str) Default gas for cathode (only relevant if `cathode_flow_module` is utilized)
        Default: 'air'
    :return: None
    """
```

```Python3
def switch_open_circuit(self) -> None:
    """
    Requires `electrical_control_module`
    Switches to open circuit condition.
    :return: None
    """
```

```Python3
def avg_V(self, duration: float) -> float:
    """
    Requires `electrical_control_module`
    Calculates the average voltage measured during a given duration.
    :param duration: (float) The duration for the averaging process.
    :return: (float) The average voltage
    """
```


## Electrolyzer-only Functions Requiring `electrical_control_module`

```Python3
def set_gas_pressure_anode(self, gas_pressure: float) -> None:
    """
    Requires `electrical_control_module`; electrolyzer-only function
    Set the gas pressure of the anode.
    :param gas_pressure: (float) Gas pressure.
    :return: None
    """
```

```Python3
def set_gas_pressure_cathode(self, gas_pressure: float) -> None:
    """
    Requires `electrical_control_module`; electrolyzer-only function
    Set the gas pressure of the cathode.
    :param gas_pressure: (float) Gas pressure.
    :return: None
    """
```

```Python3
def reset_pressures(self) -> None:
    """
    Requires `electrical_control_module`; electrolyzer-only function
    Resets both anode and cathode pressures to 0.
    :return: None
    """
```

```Python3
def read_pressures(self) -> tuple[float, float]:
    """
    Requires `electrical_control_module`; electrolyzer-only function
    Reads the anode and cathode pressures.
    :return: (tuple[float, float]) The anode and cathode pressures
    """
```

```Python3
def read_impurities(self) -> tuple[float, float]:
    """
    Requires `electrical_control_module`; electrolyzer-only function
    Reads the anode and cathode impurity levels.
    :return: (tuple[float, float]) The anode and cathode impurity levels
    """
```


## Functions Requiring `heater_control_module`

```Python3
def read_values(self) -> list[float]:
    """
    Requires `temperature_control_module`
    Reads the temperature (or other) values. Up to 8 values.
    :return: (list[float]) List of values that were read
    """
```

```Python3
def set_values(self, temp1: float = np.nan, temp2: float = np.nan, temp3: float = np.nan, temp4: float = np.nan,
               temp5: float = np.nan, temp6: float = np.nan, temp7: float = np.nan,
               temp8: float = np.nan) -> None:
    """
    Requires `heater_control_module`
    Set the (temperature) values. Up to 8 values can be set.
    :param temp1: (float) Value 1
        Default: np.nan
    :param temp2: (float) Value 2
        Default: np.nan
    :param temp3: (float) Value 3
        Default: np.nan
    :param temp4: (float) Value 4
        Default: np.nan
    :param temp5: (float) Value 5
        Default: np.nan
    :param temp6: (float) Value 6
        Default: np.nan
    :param temp7: (float) Value 7
        Default: np.nan
    :param temp8: (float) Value 8
        Default: np.nan
    :return: None
    """
```


## Functions Requiring `backpressure_module`

```Python3
def read_back_pressures(self) -> tuple[float, float]:
    """
    Requires `electrical_control_module`; fuel cell-only function
    Reads the cathode and anode back-pressures.
    :return: (tuple[float, float]) Typle of anode and cathode back-pressures
    """
```

```Python3
def set_bkp_anode(self, pressure_setpoint: int, power: int = 4) -> None:
    """
    Requires `backpressure_module` and `anode_flow_module`
    Sets the back-pressure of the anode.
    :param pressure_setpoint: (int) The back-pressure set point of the anode
    :param power: (int) Power level of the anode
        Default: 4
    :return: None
    """
```

```Python3
def set_bkp_cathode(self, pressure_setpoint: int, power: int = 4) -> None:
    """
    Requires `backpressure_module` and `cathode_flow_module`
    Sets the back-pressure of the cathode.
    :param pressure_setpoint: (int) The back-pressure set point of the cathode
    :param power: (int) Power level of the cathode
        Default: 4
    :return: None
    """
```

```Python3
def reset_bkp_anode(self) -> None:
    """
    Requires `backpressure_module`
    Resets the back-pressure of the anode.
    :return: None
    """
```

```Python3
def reset_bkp_cathode(self) -> None:
    """
    Requires `backpressure_module`
    Resets the back-pressure of the cathode.
    :return: None
    """
```


## Functions Requiring `anode_flow_module` and/or `cathode_flow_module`

```Python3
def read_flows_anode(self) -> tuple[float, float, str | float | None]:
    """
    Requires `anode_flow_module`
    Reads the gas and water flows of the anode.
    :return: (tuple[float, float, str | float | None]) gas flow, water flow, and gas ID for the anode
    """
```

```Python3
def read_flows_cathode(self) -> tuple[float, float, str | float | None]:
    """
    Requires `cathode_flow_module`
    Reads the gas and water flows of the cathode.
    :return: (tuple[float, float, str | float | None]) gas flow, water flow, and gas ID for the cathode
    """
```

```Python3
def set_gas_flowrate_anode(self, flow: float, gas: str | None = None) -> None:
    """
    Requires `anode_flow_module`
    Sets the gas flow rate of the anode.
    :param flow: (float) The gas flow rate of the anode
    :param gas: (str | None) The gas ID
        Default: None
    :return: None
    """
```

```Python3
def set_gas_flowrate_cathode(self, flow: float, gas: str | None = None) -> None:
    """
    Requires `cathode_flow_module`
    Sets the gas flow rate of the cathode.
    :param flow: (float) The gas flow rate of the cathode
    :param gas: (str | None) The gas ID
        Default: None
    :return: None
    """
```

```Python3
def set_water_flowrate_anode(self, flow: float) -> None:
    """
    Requires `anode_flow_module`
    Sets the water flow rate of the anode.
    :param flow: (float) The water flow rate of the anode
    :return: None
    """
```

```Python3
def set_water_flowrate_cathode(self, flow: float) -> None:
    """
    Requires `cathode_flow_module`
    Sets the water flow rate of the cathode.
    :param flow: (float) The water flow rate of the cathode
    :return: None
    """
```
```Python3
def reset_gases_anode(self, default_gas: str = 'H2') -> None:
    """
    Requires `anode_flow_module`
    Resets the pressure of the anode to 0 and switches to the default gas.
    :param default_gas: (str) The default gas for the anode
        Default: 'H2'
    :return: None
    """
```

```Python3
def reset_gases_cathode(self, default_gas: str = 'air') -> None:
    """
    Requires `cathode_flow_module`
    Resets the pressure of the cathode to 0 and switches to the default gas.
    :param default_gas: (str) The default gas for the cathode
        Default: 'air'
    :return: None
    """
```

```Python3
def reset_gases(self, default_gas_anode: str = 'H2', default_gas_cathode: str = 'air') -> None:
    """
    Resets the pressure of the anode to 0 and switches to the default gas.
    :param default_gas_anode: (str) The default gas for the anode
        Default: 'H2'
    :param default_gas_cathode: (str) The default gas for the cathode
        Default: 'air'
    :return: None
    """
```

```Python3
def switch_to_Ar_anode(self) -> None:
    """
    Requires `anode_anode_module`
    Switches the anode gas to Argon
    :return: None
    """
```

```Python3
def switch_to_fuel_anode(self) -> None:
    """
    Requires `anode_anode_module`
    Switches the anode gas to fuel
    :return: None
    """
```

```Python3
def switch_to_Ar_cathode(self) -> None:
    """
    Requires `cathode_anode_module`
    Switches the cathode gas to Argon
    :return: None
    """
```

```Python3
def switch_to_O2_cathode(self) -> None:
    """
    Requires `cathode_anode_module`
    Switches the cathode gas to oxygen
    :return: None
    """
```

```Python3
def switch_to_air_cathode(self) -> None:
    """
    Requires `cathode_anode_module`
    Switches the cathode gas to air
    :return: None
    """
```


## Functions Requiring `anode_flow_module`/`cathode_flow_module` and `heater_control_module`

```Python3
def calculate_and_set_water_flowrate_anode(self, pressure: float, gasflow: float | None = None,
                                           gas: str | None = None, waterflow_delta: float = 0.0, rH: float = 100.0,
                                           temp_cell: float | None = None, verbose: bool = False) -> None:
    """
    Requires `anode_flow_module` and `heater_control_module`
    Calculates and sets the water flow rate of the anode based on specified gasflow, pressure, temperature,
    and targeted rH. This utilizes our in-house algorithm.
    :param pressure: (float) The backpressure pressure of the anode
    :param gasflow: (float) The gas pressure of the anode
        Default: None
    :param gas: (str | None) The gas ID
        Default: None
    :param waterflow_delta: (float) Any deviation added to the waterflow rate (correction term).
        Default: 0.0
    :param rH: (float) The targeted rH (in percent)
        Default: 100.0
    :param temp_cell: (float) Temperature of the cell (in Celsius)
        Default: None
    :param verbose: (bool) Whether to print out additional information.
        Default: False
    :return: None
    """
```

```Python3
def calculate_and_set_water_flowrate_cathode(self, pressure: float, gasflow: float | None = None,
                                             gas: str | None = None, waterflow_delta: float = 0.0,
                                             rH: float = 100.0, temp_cell: float | None = None,
                                             verbose: bool = False) -> None:
    """
   Requires `cathode_flow_module` and `heater_control_module`
   Calculates and sets the water flow rate of cathode based on specified gasflow, pressure, temperature,
   and targeted rH. This utilizes our in-house machine learning model.
   :param pressure: (float) The backpressure pressure of the cathode
   :param gasflow: (float) The gas pressure of the cathode
       Default: None
   :param gas: (str | None) The gas ID
       Default: None
   :param waterflow_delta: (float) Any deviation added to the waterflow rate (correction term).
       Default: 0.0
   :param rH: (float) The targeted rH (in percent)
       Default: 100.0
   :param temp_cell: (float) Temperature of the cell (in Celsius)
       Default: None
   :param verbose: (bool) Whether to print out additional information.
       Default: False
   :return: None
   """
```


## Functions Requiring `electrical_control_module` and `load_hardware`

```Python3
def OCV(self, duration: float, skipable: bool = False) -> None:
    """
    Requires `electrical_control_module` and `load_hardware`
    Put system in open curcuit voltage condition for the specified duration.
    :param duration: (float) Duration in seconds
    :param skipable: (bool) Whether to be able to skip the duration (by pressing `end` key on the keyboard)
        Default: False
    :return: None
    """
```

```Python3
def FC_mode_CC(self, duration: float, load_current: list[float] | float, current_range: int = 40,
               final_OCV: bool = True, skipable: bool = False, skip_voltage: float | None = None) -> None:
    """
    Requires `electrical_control_module` and `load_hardware`
    Fuel cell constant current mode. Go through list of load currents that are held for specified duration.
    :param duration: (float) Duration in seconds
    :param load_current: (list[float] | float) List of load currents
    :param current_range: (int) Current range
        Default: 40
    :param final_OCV: (bool) Whether to end in OCV
        Default: True
    :param skipable: (bool) Whether to be able to skip current steps (by pressing `end` key on the keyboard)
        Default: False
    :param skip_voltage: (float | None) Voltage that determines the skipping trigger
        Default: None
    :return: None
    """
```

```Python3
def FC_mode_CV(self, duration: float, load_voltage: list[float] | float, current_range: int = 1,
               final_OCV: bool = True, skipable: bool = False) -> None:
    """
    Requires `electrical_control_module` and `load_hardware`
    Fuel cell constant current mode. Go through list of load currents that are held for specified duration.
    :param duration: (float) Duration in seconds
    :param load_voltage: (list[float] | float) List of load voltages
    :param current_range: (int) Current range
        Default: 1
    :param final_OCV: (bool) Whether to end in OCV
        Default: True
    :param skipable: (bool) Whether to be able to skip current steps (by pressing `end` key on the keyboard)
        Default: False
    :return: None
    """
```


## Functions Requiring `electrical_control_module` and `potentiostat_hardware`

```Python3
def cyclic_voltammetry(self, V_start: float, V1: float, V2: float, V_end: float, scan_rate: float, n_cycles: int,
                       sample_rate: float = 0.01, plot: bool = True) -> None:
    """
    Requires `electrical_control_module` and `potentiostat_hardware`
    Cyclic voltammetry protocol from starting to ending voltage with specified number of cycles
    between V1 and V1 for n_cycles at specified scan rate and sample rate.
    :param V_start: (float) Starting voltage
    :param V1: (float) Voltage 1
    :param V2: (float) Voltage 2
    :param V_end: (float) Ending voltage
    :param scan_rate: (float) Scan rate
    :param n_cycles: (int) Number of cycles
    :param sample_rate: (float) Sample rate
        Default: 0.01
    :param plot: (bool) Whether to generate a plot at the end
        Default: True
    :return: None
    """
```

```Python3
def CV_potentiostat(self, voltages: list, hold_time: float = 300.0, plot: bool = True) -> None:
    """
    Requires `electrical_control_module` and `potentiostat_hardware`
    CV mode with potentiostat for specified list of voltages at specified holding time.
    :param voltages: (list) List of voltages
    :param hold_time: (float) Hold time in seconds
        Default: 300.0
    :param plot: (bool) Whether to generate a plot at the end
        Default: True
    :return: None
    """
```

```Python3
def squarewave_potentiostat(self, voltage1: float, voltage2: float, hold_time_1: float = 3.0,
                            hold_time_2: float = 3.0, ascend_time: float = 0.1, descend_time: float = 0.1,
                            n_cycles: int = 10000, plot: bool = True) -> None:
    """
    Requires `electrical_control_module` and `potentiostat_hardware`
    Squarewave potentiostat mode between two states of defined voltages, holding times for specified
    number of cycles with ascending and descending times.
    :param voltage1: (float) Voltage at state 1
    :param voltage2: (float) Voltage at state 2
    :param hold_time_1: (float) Holding time at state 1 in seconds
        Default: 3.0
    :param hold_time_2: (float) Holding time at state 2 in seconds
        Default: 3.0
    :param ascend_time: (float) Time it takes to ascend, in seconds
        Default: 0.1
    :param descend_time: (float) Time it takes to descend, in seconds
        Default: 0.1
    :param n_cycles: (int) Number of squarewave cycles
        Default: 10000
    :param plot: Whether to generate a plot at the end
        Default: True
    :return: None
    """
```

## Function Requiring `potentiostat_hardware`, `load_hardware`, and `electrical_control_module`

```Python3
def EIS_load(self, base_current: float, perturbation_percentage: float, starting_frequency: float = 10000.0,
             ending_frequency: float = 20.0, points_per_decade: int = 20,
             base_current_wait: float = 5.0, plot: bool = True) -> None:
    """
    Requires `potentiostat_hardware`, `load_hardware`, and `electrical_control_module`
    EIS mode with load and potentiostat for specified base current, perturbation percentage,
    starting/ending frequency, base current wait time and points per decade.
    :param base_current: (float) Base current
    :param perturbation_percentage: (float) Perturbation percentage
    :param starting_frequency: (float) Starting frequency in Hertz
        Default: 10000.0
    :param ending_frequency: (float) Ending frequency in Hertz
        Default: 20.0
    :param points_per_decade: (int) Number of points per decade to be collected
        Default: 20
    :param base_current_wait: (float) Wait time for base current, in seconds
        Default: 5.0
    :param plot: Whether to generate a plot at the end
        Default: True
    :return: None
    """
```


## Function Requiring `electrical_control_module`, `load_hardware`, `anode_flow_module`, and `cathode_flow_module`

```Python3
def starvation_loop(self, cycles: int, duration1: float, anode_flow1: float, cathode_flow1: float,
                    load_current1: float, duration2: float, anode_flow2: float, cathode_flow2: float,
                    load_current2: float, Vskip: float, final_OCV: bool = True) -> None:
    """
    Requires `electrical_control_module`, `load_hardware`, `anode_flow_module`, and `cathode_flow_module`
    Starvation loop procedure for specified number of cycles between two states,
    each with a specified duration, anode/cathode flows and a load current
    :param cycles: (int) Number of cycles
    :param duration1: (float) Duration of state 1
    :param anode_flow1: (float) Anode flow of state 1
    :param cathode_flow1: (float) Cathode flow of state 1
    :param load_current1: (float) Load current of state 1
    :param duration2: (float) Duration of state 2
    :param anode_flow2: (float) Anode flow of state 2
    :param cathode_flow2: (float) Cathode flow of state 2
    :param load_current2: (float) Load current of state 2
    :param Vskip: (float) Voltage that determines skipping condition
    :param final_OCV: (bool) Whether to end in OCV
        Default: True
    :return: None
    """
```