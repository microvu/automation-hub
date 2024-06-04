---
layout: page
title: "First Program"
---

This guide will walk you through creating a program.

For this guide it is assumed:
- A Micro-Vu machine is connected to the computer
- InSpec is open and the machine has been initialized.
- Automated control has been enabled
- An InSpec program should be saved and ready to run.

To enable automated control of InSpec, navigate to Tools>Automation>Enable Automated Control.
![]({{ site.first_program_img }}/enable-control.png)

First create a new device to connect to InSpec.
Give it a name and leave the _Host_ as `localhost` since the machine is connected to the same computer.
For _Directory_, browse to the folder containing InSpec programs.
![]({{ site.first_program_img }}/devices.png)
<br>

Switch to the _Variables_ tab and create a new variable.
Name it `count` since this program will count the number of times the InSpec program completes.
Set the _Type_ to `Uint32` and the _Initial Value_ to 0. Be sure to press _Save_ for the changes to be applied.
![]({{ site.first_program_img }}/variables.png)
<br>

Nearly all programs will use the _OnStart_ event.
It serves as the starting point of the routine.
The event is emitted exactly once so it is safe to do initialization with it.
To add the _OnStart_ event to the routine simply drag it from the command panel on the right or use the shortcut `ss`.
![]({{ site.first_program_img }}/step-1.png)
<br>

Next we will run a part program in InSpec.
Drag the _RunProgram_ command from the side panel or use the shortcut `rr`.
We want the InSpec program to run as soon as the routine is started so drag the output of _OnStart_ to the trigger of _RunProgram_.
Select the device from the dropdown and input the name of the InSpec program below it.
![]({{ site.first_program_img }}/step-2.png)
<br>

This part is optional, but if any feature fails, is out of tolerance, or the machine has a fault we'll stop the current InSpec program immediately.
Drag the _OnFeatureFailed_, _OnOutOfTolerance_, and _OnFault_ output events to the trigger of _StopRunning_.
Don't forget to set the device too.
![]({{ site.first_program_img }}/step-3.png)
<br>

Now add an _Arithmetic_ command (shortcut: `ar`).
We will increment `count` every time the InSpec program successfully finishes.
Connect the _OnFinished_ output to the trigger of _Arithmetic_.
Set the operator to `+` and the second operand to `1`.
![]({{ site.first_program_img }}/step-4.png)
<br>

To get the value of the `count` variable, add the _GetVariable_ command (shortcut: `gv`).
Select the `count` variable from the dropdown.
Connect the _Value_ output to the first operand of _Arithmetic_.
Notice that we are using the passive output of GetVariable.
This is because we are trying to change the value of `count` so using the _OnChange_ event to trigger the change would not work.
![]({{ site.first_program_img }}/step-5.png)
<br>

Now that the correct value is being calculated we need to store it back in the `count` variable.
Add a _SetVariable_ command (shortcut: `sv`) and set the variable to `count`.
Connect the _Result_ of the _Arithmetic_ command to the _Value_ input of _SetVariable_.
Now when the program is finished `count` will be incremented by 1.
However, the InSpec program will only run one time so the most `count` can be is one.
![]({{ site.first_program_img }}/step-6.png)
<br>

To allow the program to keep running we need to create a cycle.
Add a _Wait_ command (shortcut: `ww`), set the time to `1000` ms or greater (to add some delay between cycles).
Connect the _OnChange_ output to the trigger of _Wait_ and the output of Wait to the trigger of _RunProgram_.
Now when either the routine is started or the variable `count` is changed the InSpec program will run.
Since `count` is only incremented when the InSpec program successfully completes, the cycle will continue as long as parts pass.
If any fail the loop will stop.
![]({{ site.first_program_img }}/step-7.png)
<br>

The routine is now complete.
Press _Start_ at the top of the program to see it in action.

Since the InSpec program takes time to complete, the _RunProgram_ command will stay yellow as long is the program is running.
![]({{ site.first_program_img }}/run-1.png)

If the part was measured successfully, you should now see the _Wait_ command lit up.
The other commands complete instaneously so you may not see them light up.
![]({{ site.first_program_img }}/run-2.png)
The `count` variable should now be equal to 1 and the program will run again.
![]({{ site.first_program_img }}/run-3.png)
