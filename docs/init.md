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


