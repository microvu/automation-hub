---
layout: default
title: "Getting Started"
---

# Getting Started

The Automation Hub greatly simplifies automating Micro-Vu measuring machines.
It supports controlling and synchronizing multiple machines at once using an intuitive visual programming interface.
For integrating with other devices, the program can accept ModbusTCP/IP connections and allow read and write access to registers.

---

- [Network Topology](#network-topology)
- [User Interface](#user-interface)
- [Config Panel](#config-panel)
  - [Devices](#i-devices)
  - [Variables](#ii-variables)
  - [Log](#iii-log)
- [Editor and Command Panel](#editor-and-command-panel)

---

## Network Topology

To get started it is helpful to understand where this program fits in.
There are many possible configurations to fit the need of any automation setup.

- In the simplist case, one machine is controlled and the Automation Hub is ran on same computer as InSpec.
  This setup is useful for running a sequence of InSpec programs automatically or changing programs dynamically based on feature measurements.  
  ![]({{ site.gettting_started_img }}/topology-1.png)

- A slightly more complicated case, here multiple machines are controlled.
  The Automation Hub can run on a separate computer or on one running InSpec.
  ![]({{ site.gettting_started_img }}/topology-2.png)

- This is the most advanced setup.
  The Automation Hub can act as a slave for a PLC or other Modbus enabled devices.
  Multiple Micro-Vu machines can be integrated with robots and other external devices to create a fully automated cell.
  The Automation Hub handles all the complications of controlling the machines internally while exposing a simple, customizable interface to the user.

![]({{ site.gettting_started_img }}/topology-3.png)

To enable automated control of InSpec, navigate to Tools>Automation>Enable Automated Control.
![]({{ site.first_program_img }}/enable-control.png)

## User Interface

The interface is split into 3 main sections:

1. Config Panel
   ![]({{ site.gettting_started_img }}/config-panel-highlight.png)
2. Editor
   ![]({{ site.gettting_started_img }}/editor-highlight.png)
3. Command Panel
   ![]({{ site.gettting_started_img }}/command-panel-highlight.png)

## Config Panel

The config panel is located on the left side.
It usually the first thing used when starting a new program.
It has 3 different tabs that can be clicked to change between different panels.

![]({{ site.gettting_started_img }}/config-panel-tabs.png)

#### I. Devices

The first tab is the _Devices_ tab.

_Devices_ are for connecting to remote PCs with Micro-Vu machines attached.
The _Name_ is just for identifying the device within the program.
_Host_ is the hostname of the PC on the network.
An IP address can also be entered.
_Directory_ should be an absolute path to the directory containing InSpec programs.
The InSpec programs need to be accessible client-side so it is highly recommended to use a network drive.

![]({{ site.gettting_started_img }}/devices-1.png)

<br>
_Servers_ allow PLCs or other Modbus enabled devices to connect to the Automation Hub.
Once connected, the servers can read and write to Modbus registers like with any other device.
To use the register values within a routine, a variable that is bound to a register address is needed.

![]({{ site.gettting_started_img }}/devices-2.png)

#### II. Variables

Variables are used to store values. They have a _Name_, a _Type_, and an _Initial Value_.
The name should be descriptive of what the value represents.
The type determines the size and layout of the variable's memory; the range of values that can be stored within that memory; and the set of operations that can be applied to the variable.
The available types are:

- _Boolean_: Stores either value True or False
- _Float32_: A single-precision floating point value
- _Float64_: A double-precision floating point value
- _Int16_: Signed integer value -32,768 to 32,767
- _Int32_: Signed integer value -2,147,483,648 to 2,147,483,647
- _Uint16_: Unsigned integer value 0 to 65,535
- _Uint32_: Unsigned integer value 0 to 4,294,967,295
- _String_: Text or character values

The correct type will depend on the application but for a general rule of thumb use:

- _Boolean_ when the variable can only be True or False, or On or Off
- _Int32_ for counting
- _Float64_ for coordinates
- _String_ for text and program names

![]({{ site.gettting_started_img }}/variables-1.png)

Variables can be given initial values.
The actual value is shown just below.
If no initial value is given the variable is given the value of `undefined`.
![]({{ site.gettting_started_img }}/variables-2.png)

##### Advanced variable options

For advanced control, variables can be bound to a Modbus register.
When a PLC or other device connects to the Automation Hub it can read and write to a set of registers.
When a variable is bound to a register address, anytime the register value is changed the variable value is changed too.
This is true in reverse as well, if the variable is changed in the routine the registers will update and a PLC can read the new value.
There are two sets of registers.
_Boolean_ values are stored in one while all other values are stored in the other.
Both sets have 1024 registers, with valid address ranges of (0-1023).
All servers share the same registers.

![]({{ site.gettting_started_img }}/variables-3.png)

![]({{ site.gettting_started_img }}/variables-4.png)

#### III. Log

The _Log_ shows warnings and erros that may prevent the routine from running correctly.
Warnings are shown with a yellow triangle and allow the routine to continue running.
Errors are shown with a red circle and the routine will immediately stop when one is encountered.

![]({{ site.gettting_started_img }}/log-1.png)

### Editor and Command Panel

The editor window is where the routine is created.
The routine captures all the logic for running the program.
Commands can be dragged in from the _Command Panel_ on the right.

Commands can also be added using keyboard shortcuts

- `ss` OnStart
- `gs` GetMachineStatus
- `gv` GetVariable

- `cr` ContinueRunning
- `gt` GoToLoadPosition
- `rf` ReturnFromLoadPosition
- `rr` RunProgram
- `sr` StopRunning
- `gf` GetFeatureProperty
- `ac` PlaceManualPoint
- `dt` DriveToPosition

- `sv` SetVariable
- `ff` If
- `sw` Switch
- `aw` AwaitMultiple
- `bc` BooleanComparison
- `cc` NumericComparison
- `ar` Arithmetic
- `ww` Wait
- `zz` Stop

Left Mouse Button (`LMB`) can be used to select commands in the _Editor_.
Selected commands are highlighted in yellow.

`Ctrl+LMB` can be used to select multiple commands by clicking on each one.

Right Mouse Button (`RMB`) can be used to box select multiple commands quickly.

Selected commands can be moved by holding down `LMB` and draging.

Also, selected commands can be deleted using the `Delete` key and duplicated using `Ctrl+d`.

The window can be panned using the Middle Mouse Button (`MMB`) and zoomed using the scroll wheel.

`Home` can be used to center the view on all the commands.

Click and drag with the `LMB` from either input or output to another to create a link.
While dragging only the invalid inputs/outputs will be hidden.

![]({{ site.gettting_started_img }}/editor-drag.png)

To remove links double click with the `LMB` to remove all links connected to the input/output.

![]({{ site.gettting_started_img }}/editor-double-click.png)

`Ctrl+r` can be used to start and stop the routine.

`Ctrl+s` will save the current program.

`Ctrl+shift+s` to save the current program as a new file.

`Ctrl+n` to create a new program.

`Ctrl+o` to open an existing program.
