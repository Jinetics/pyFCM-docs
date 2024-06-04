---
hide:
  - toc
---

# Hardware Customization Through Python Interface

Jinetics enables the user to use their own custom hardware and supplies template Python interfaces. 
The user then needs to complete the necessary class functions for their specific hardware/use case.

One example is given below for the `load_hardware`. 
The user needs to complete the functions `__init__` (any hardware initialization has to go here), 
`pre_current` (anything the load has to do before setting a current), ...

Functions can also remain empty if, for example, the hardware does not need to do anything before setting the current. 
In that case just add `pass` inside the function.

```Python3
# Use Python 3.10
import time
import pyvisa

class CustomLoad:

    def __init__(self, address: str = 'test', verbose: bool = False):
        """
        Code in here what is required to initialize the load hardware.
        :param address: Address to connect to load, something like 'USB0::....'
        :param verbose: Variable to handle if text is printed out or not (optional)
        """
        # Code here.
        rm = pyvisa.ResourceManager()
        self.load = rm.open_resource(address)
        if verbose:
            print('Load successfully initialized.', flush=True)

    def pre_current(self, current_range: int = 40) -> None:
        """
        Put any code that needs to be run to prepare the load before setting currents.
        :param load_current_range: Variable that determines the load range (optional variable)
        :return:
        """
        # Code here
        self.load.write(':SOUR:FUNC CURR')  # EXAMPLE

    def set_current(self, current: float) -> None:
        """
        Put code that sets the current of the load.
        :param current: Current set point
        :return:
        """
        # Code here
        self.load.write(':SOUR:CURR:LEV:IMM {:.2f}'.format(current))  # EXAMPLE

    def post_current(self):
        """
        Put any code that needs to be run at the end of current measurements (e.g. turn off load).
        :return:
        """
        # Code here

    def pre_bias(self) -> None:
        """
        Put any code that needs to be run to prepare the load before setting a voltage.
        :return:
        """
        # Code here
        self.load.write(':SOUR:FUNC VOLT')  # EXAMPLE

    def set_voltage(self, voltage: float) -> None:
        """
        Put code that sets the bias of the load.
        :param voltage:
        :return:
        """
        # Code here
        self.load.write(':SOUR:VOLT:LEV:IMM {:.2f}'.format(voltage))  # EXAMPLE

    def post_bias(self) -> None:
        """
        Put any code that needs to be run at the end of voltage measurements (e.g. turn off load).
        :return:
        """
        # Code here
        self.load.write(':SOUR:SENS 0')  # EXAMPLE

    def turn_on(self) -> None:
        """
        Code to turn on the load device.
        :return:
        """
        # Code here
        self.load.write(':SOUR:INP:STAT 1')  # EXAMPLE

    def turn_off(self) -> None:
        """
        Code to turn off the load device.
        :return:
        """
        # Code here
        self.load.write(':SOUR:INP:STAT 0')  # EXAMPLE


if __name__ == '__main__':
    # Test your code by running this script
    load = Load('USB0::.....')
    # Testing CC mode:
    load.pre_current(current_range=40)
    load.set_current(0.5)
    time.sleep(5)  # Check that current was set correctly
    load.post_current()
    time.sleep(5)  # Check that load is idle/turned off

    # Testing CV mode:
    load.pre_bias()
    load.set_voltage(0.5)
    time.sleep(5)  # Check that voltage was set correctly
    load.post_bias()
    time.sleep(5)  # Check that load is idle/turned off

```

